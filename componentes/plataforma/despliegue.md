---
title: Proceso de despliegue de CONVIDA
description: 
published: true
date: 2026-03-26T15:17:25.218Z
tags: 
editor: markdown
dateCreated: 2026-03-25T18:49:19.837Z
---

# Resumen

El despliegue de CONVIDA está distribuido entre varios repositorios de aplicación y un repositorio central de infraestructura llamado `convida_deployment`.

Los repositorios que participan directamente en el proceso son:

- `abejopolis`
- `libado`
- `Editor_V2`
- `honeycomb`
- `convida_deployment`

A nivel general, el flujo esperado es:

1. Se realiza un cambio en alguno de los repositorios de aplicación.
2. Al hacer `push` a la rama `main`, se ejecuta un workflow de GitHub Actions llamado **Trigger for deploy**.
3. Ese workflow notifica al repositorio `convida_deployment`.
4. En `convida_deployment` se valida el estado del código, se solicita aprobación manual y finalmente se construyen y levantan los contenedores en el servidor **self-hosted**.

# Repositorios involucrados

## 1. Repositorios de aplicación

Cada uno de estos repositorios contiene un workflow con la misma estructura base:

- **[Abejopolis](https://github.com/convidauam/abejopolis)**
- **[Libado](https://github.com/convidauam/libado)**
- **[Editor_V2](https://github.com/convidauam/Editor_V2)**
- **[Honeycomb](https://github.com/convidauam/honeycomb)**

Todos cuentan con un workflow llamado **Trigger for deploy** que:

- Se ejecuta en `push` sobre `main`
- Corre en _ubuntu-latest_
- Realiza una petición `POST` a la API de GitHub
- Dispara un evento sobre el repositorio `convida_deployment`

### Eventos enviados por cada repositorio

- `abejopolis` envía `trigger-from-abejopolis`
- `libado` envía `trigger-from-libado`
- `Editor_V2` envía `trigger-from-editor`
- `honeycomb` envía `trigger-from-honeycomb`

## 2. Repositorio de infraestructura: [convida_deployment](https://github.com/convidauam/convida_deployment)

Este repositorio concentra la lógica de despliegue en producción. Ahí se encuentran:

- `compose.yaml`
- Configuración de `nginx`
- Certificados SSL
- Workflow de despliegue
- Tareas auxiliares para pruebas locales definidas en `mise.toml`

# Flujo detallado

## Paso 1. Cambio en un repositorio de aplicación

Cuando se hace `push` a `main` en cualquiera de los repositorios de aplicación, GitHub Actions ejecuta el workflow **Trigger for deploy**.

El workflow no construye imágenes ni despliega directamente. Su única responsabilidad es **notificar al repositorio de despliegue** usando la API de GitHub.

Esto desacopla el código de aplicación del pipeline de infraestructura.

## Paso 2. Recepción del evento en `convida_deployment`

El repositorio `convida_deployment` es el encargado de orquestar el despliegue completo.

En el workflow principal, el pipeline está dividido en tres jobs:
1. `build-convida`
2. `manual-approval`
3. `deploy-convida`

La intención operativa del flujo es:
- Preparar el checkout del repositorio de despliegue en el servidor
- Sincronizar archivos al directorio de trabajo
- Verificar que existan los Dockerfiles necesarios
- Esperar aprobación manual
- Configurar secretos y certificados
- Reconstruir y levantar toda la plataforma con Docker Compose

## Paso 3. Job `build-convida`

Este job corre sobre un *runner self-hosted*.

### Responsabilidades

#### Obtener o actualizar `convida_deployment`
El pipeline:
- Crea `~/Convida/` si no existe
- Si ya existe el repositorio `~/Convida/convida_deployment/.git`, hace `git pull`
- Si no existe, clona el repositorio
- Después sincroniza el contenido desde `~/Convida/convida_deployment/` hacia `~/Convida/` usando `rsync`

Esto deja en la raíz de trabajo los archivos operativos necesarios para el despliegue.

#### Validar repositorios y Dockerfiles
El job revisa que existan los Dockerfiles de:
- **abejopolis**
- **libado**
- **editor_convida**
- **honeycomb**

Si falta alguno, el pipeline termina con error.

## Paso 4. Aprobación manual

Después del job de preparación, el workflow ejecuta `manual-approval`.

Cómo funciona:
- Corre en _ubuntu-latest_
- Depende de `build-convida`
- Usa la acción `trstringer/manual-approval@v1`
- Asigna la aprobación al usuario designado [actualmente [pascalin (David Suárez)](https://github.com/pascalin)]
- Requiere al menos 1 aprobación

Esto introduce una compuerta humana antes del despliegue real en producción.

## Paso 5. Job `deploy-convida`

Una vez aprobada la ejecución, el pipeline continúa en el *runner self-hosted*.

### Configuración de certificados SSL y secretos

El workflow toma de GitHub Secrets:
- `SSL_CERT`
- `SSL_INTERMEDIATE`
- `SSL_KEY`
- `BACKEND_AUTH_SECRET`

Después:
- Verifica si los archivos de certificado ya existen en `~/Convida/reverse_proxy/certs`
- Si faltan, los reconstruye decodificando el contenido en base64
- Modifica `production.ini` de honeycomb para establecer `auth.secret`

### Limpieza de contenedores e imágenes

Antes de reconstruir el stack, el pipeline ejecuta:
- `docker stop $(docker ps -aq)`
- `docker rm $(docker ps -aq)`
- `docker rmi $(docker images -q)`

> Es una limpieza agresiva del host Docker para asegurar una reconstrucción completa.
{.is-info}


### Build y arranque de la plataforma

Finalmente se ejecuta:

```bash
sudo docker-compose --project-name convida --file "$BASE/compose.yaml" build --no-cache
sudo docker-compose --project-name convida --file "$BASE/compose.yaml" up --detach
```

Con esto se reconstruyen las imágenes y se levanta el stack de producción.

# Servicios desplegados en producción

El stack incluye los siguientes servicios:
- **frontend**
- **backend**
- **libado**
- **abejopolis**
- **anubis**
- **reverse-proxy**

Todos se conectan a la red Docker `convida_network`.

## Función de cada servicio

- **frontend**

    Construye el editor desde editor_convida/Dockerfile.

    Incluye argumentos de build para:

        - `REACT_APP_START_URL`
        - `REACT_APP_API_URL`

- **backend**

    Construye el backend desde `honeycomb/Dockerfile`.

- **libado**

    Construye el juego o módulo de Libado desde `libado/Dockerfile`.

- **abejopolis**
    
    Construye el juego o módulo de Abejopolis desde `abejopolis/Dockerfile`.

- **anubis**

    Usa la imagen actualizada de [Anubis](https://anubis.techaro.lol/) cómo capa de protección HTTP.

- **reverse-proxy**

    Construye el contenedor Nginx a partir de `reverse_proxy/Dockerfile` y expone:
    - `80:80`
    - `443:443`

    Este servicio depende de todos los anteriores.

## Rol de Nginx y Anubis

La configuración de `reverse_proxy/conf.d/nginx.conf` indica que Nginx actúa como punto de entrada HTTPS para [convida.cua.uam.mx](https://convida.cua.uam.mx/).

### Comportamiento principal
- Redirección de HTTP a HTTPS
- Terminación TLS
- Proxy a frontend, backend, libado y abejopolis
- Integración con anubis mediante auth_request

### Rutas principales
- / → frontend
- /backend/ → backend
- /libado/ → libado
- /abejopolis/ → abejopolis
- /.within.website/ → anubis

### Protección con Anubis

Antes de servir contenido, Nginx hace una validación con:

```nginx
auth_request /.within.website/x/cmd/anubis/api/check;
error_page 401 403 =200 /.within.website/?redir=$request_uri;
```

Eso significa que Anubis funciona como un filtro previo para acceso o mitigación de tráfico automatizado, antes de pasar la solicitud al servicio final.

# Preparación en entorno local

El archivo `mise.toml` define tareas auxiliares para preparar entornos de desarrollo o prueba.

Tareas relevantes
- Clonar o actualizar:
    - abejopolis
    - libado
    - editor_convida
    - honeycomb
- Definir un secreto de autenticación local en `honeycomb/development.ini`
- Construir imágenes Docker
- Levantar contenedores
- Limpiar directorios locales

Esto no forma parte directa del despliegue productivo del workflow, pero si prepara el entorno de trabajo para pruebas y validación.

> Se necesita de Docker corriendo en el sistema local para que todas las tareas de *mise* funcionen correctamente.
> En windows es recomendable correr los comandos desde la terminal de git.
{.is-success}

# Secretos requeridos

Para que el pipeline funcione correctamente, se necesitan al menos los siguientes secretos:

En los repositorios de aplicación
- `GHP_TOKEN`

Se usa para autenticar la llamada a la API de GitHub que notifica a `convida_deployment`.

[Administración de tokens de acceso personal - Documentación de GitHub](https://docs.github.com/es/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)

En `convida_deployment`
- `GHP_TOKEN`
- `SSL_CERT`
- `SSL_INTERMEDIATE`
- `SSL_KEY`
- `BACKEND_AUTH_SECRET`

[Uso de secretos en Acciones de GitHub - Documentación de GitHub](https://docs.github.com/es/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets)

# Diagrama lógico del proceso

![deploy-diagram.png](/deploy-diagram.png){.align-center}

# Observaciones

1. El despliegue está centralizado.
    
    Los repositorios de aplicación no despliegan por sí mismos; únicamente notifican al repositorio central.

2. El servidor de despliegue usa un *runner self-hosted*

    Esto implica que el runner tiene acceso directo al sistema de archivos, certificados, Docker y configuración del host.

3. El despliegue actual reconstruye todo

    El job de producción detiene y elimina todos los contenedores e imágenes Docker antes de volver a construir. Esto simplifica el estado del entorno, pero también aumenta el impacto de cada despliegue.

4. Hay una aprobación manual obligatoria

    No existe despliegue automático directo a producción después del `push`; siempre hay una intervención humana intermedia.

5. Nginx y Anubis forman parte del despliegue

    La publicación del sistema no solo depende de las aplicaciones, sino también del proxy reverso y de la capa de protección de tráfico.
    
# Responsable
- [Jorge Ángel Juárez Vázquez](https://github.com/jorge-jrzz)