---
title: Contenerización
description: Contenerización de los diferentes componentes
published: true
date: 2025-09-10T00:20:10.448Z
tags: plataforma, docker
editor: markdown
dateCreated: 2025-09-10T00:20:10.448Z
---

# Honeycomb

## Introducción a las Herramientas Utilizadas

En este proyecto, utilizamos varias herramientas modernas para facilitar la configuración, el desarrollo y la ejecución del entorno. A continuación, una breve introducción a cada una a nivel introductorio:

- **mise**: Es una herramienta de gestión de entornos de proyecto que automatiza la instalación de dependencias y la ejecución de tareas. Similar a herramientas como asdf o direnv, permite confiar en el entorno del proyecto y ejecutar comandos predefinidos de manera reproducible. Se recomienda su uso para simplificar el flujo de trabajo.
  
- **uv**: Es una herramienta desarrollada por Astral para gestionar entornos virtuales de Python de forma rápida y eficiente. Se integra bien con mise y ayuda en la instalación de paquetes, asegurando compatibilidad y velocidad en el manejo de dependencias.

- **Docker**: Es una plataforma de contenedores que permite empaquetar la aplicación y sus dependencias en un contenedor aislado. Esto asegura que el proyecto se ejecute de manera consistente en cualquier máquina, independientemente del sistema operativo subyacente.

Estas herramientas ayudan a mantener un entorno de desarrollo limpio, reproducible y portátil, reduciendo problemas comunes como conflictos de versiones.

## Inicio con mise (Recomendado)

La forma más sencilla de configurar y ejecutar este proyecto es utilizando [mise](https://mise.jdx.dev/getting-started.html#installing-mise-cli) y [uv](https://docs.astral.sh/uv/getting-started/installation/#installation-methods).

Primero, confía en el entorno del proyecto:

```bash
mise trust
```

Puedes explorar las tareas disponibles con:

```bash
mise tasks
```

### Tareas Principales

- **Configurar el entorno:**

    Instala todas las dependencias (incluyendo grupos opcionales definidos en el proyecto):

    ```bash
    mise setup
    ```
    
- **Iniciar el servidor de desarrollo:**
    
    Ejecuta el proyecto localmente utilizando el lanzador de desarrollo personalizado:
    
    ```bash
    mise dev
    ```
    
- **Ejecutar pruebas:**
    
    Lanza la suite de pruebas con pytest:

    ```bash
    mise test
    ```

- **Construir y ejecutar con Docker:**

    Construye la imagen Docker del proyecto y inicia un contenedor:

    ```bash
    mise docker
    ```

Eso es todo — con mise, todo está automatizado y reproducible.

---

## Alternativa Manual (sin mise)

1. Cambia al directorio del proyecto (donde se encuentran este README y `setup.py`):

    ```bash
    cd honeycomb
    ```

2. Crea un entorno virtual de Python:

    ```bash
    python3 -m venv env
    ```

3. Actualiza las herramientas de empaquetado:

    ```bash
    env/bin/pip install --upgrade pip setuptools
    ```

4. Instala el proyecto en modo editable con requisitos de pruebas:

    ```bash
    env/bin/pip install -e ".[testing]"
    ```

5. Ejecuta las pruebas del proyecto:

    ```bash
    env/bin/pytest
    ```

6. Ejecuta el proyecto:

    ```bash
    env/bin/pserve development.ini
    ```
---

## Ejecutar con Docker (independiente)

1. Ve al directorio del proyecto.
    
2. Construye la imagen Docker:
    ```bash
    docker build -t "honeycomb:latest" .
    ```

3. Ejecuta el contenedor:

    ```bash
    docker run -p 6543:6543 --rm honeycomb:latest
    ```

4. Abre tu navegador en: [http://127.0.0.1:6543](http://127.0.0.1:6543)