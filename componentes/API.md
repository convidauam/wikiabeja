---
title: API
description: API CONVIDA
published: true
date: 2026-10-02T18:58:34.048Z
tags: api
editor: markdown
dateCreated: 2025-07-22T01:01:03.085Z
---

# API

## API honeycomb

La API Honeycomb es el punto principal de acceso a la estructura de un Convida particular representada como grafos con nodos y aristas. Cada Convida (o honeycomb) es un nodo raíz con celdas (nodos hijos) conectadas mediante aristas, en donde cada celda puede contener más nodos o algún otro tipo de contenido.

### ¿Qué tecnologías se ocuparon?

* **Python 3.x**
Lenguaje base.

* **Pyramid**
 Framework web principal para crear la aplicación WSGI.

* **Cornice**
 Extensión para Pyramid que facilita la creación de APIs REST.

* **ZODB**
Base de datos orientada a objetos usada para almacenar la jerarquía de honeycombs, celdas y sus relaciones.

* **pyramid_zodbconn**
Integración de ZODB con Pyramid.

* **Jinja2**
Motor de plantillas para renderizar vistas HTML (además de la API JSON).
### Endpoints principales
1. **Listar honeycombs**
GET <kbd>/api/v1/honeycombs</kbd>
Devuelve la lista de honeycombs registrados.

2. **Obtener un honeycomb específico**
GET <kbd>/api/v1/honeycombs/{name}</kbd>
Devuelve la representación en grafo de un honeycomb: nodo raíz, nodos hijos y aristas.

---
### Ejemplo de respuesta (200)
```json
{
  "honeycombs": [
    {
      "id": "default",
      "title": "Honeycomb principal",
      "icon": "https://example.com/icon.png"
    },
    {
      "id": "juegos",
      "title": "Panal de juegos",
      "icon": "https://example.com/game-icon.png"
    }
  ]
}
```
#### Descripción: 
Devuelve la estructura de un honeycomb específico, incluyendo:
- Nodo raíz (el honeycomb mismo).
- Nodos hijos distribuidos en círculo alrededor del nodo raíz.
- Aristas que conectan el nodo raíz con cada hijo.
- Parámetros de ruta
- name (string, requerido) → El nombre único del honeycomb.

### Ejemplo real (200) - panal de juegos
```json
{
  "id": "e93faa5a-f290-5c0e-9c11-a8529592f7ae",
  "title": "Panal de Juegos",
  "nodes": [
    {
      "id": "e93faa5a-f290-5c0e-9c11-a8529592f7ae",
      "data": {
        "label": "Panal de Juegos",
        "themeColor": "root",
        "url": "http://localhost:6543/panal-de-juegos/",
        "icon": null
      },
      "position": {
        "x": 0,
        "y": 0
      },
      "type": "custom",
      "width": 200,
      "height": 80
    },
    {
      "id": "e883b554-6fef-432d-9cee-b3013b96ebf2",
      "data": {
        "label": "Juego de la Serpiente",
        "themeColor": "default",
        "url": "http://localhost:6543/panal-de-juegos/juego-de-serpiente/",
        "icon": null
      },
      "position": {
        "x": 300,
        "y": 0
      },
      "type": "custom",
      "width": 152,
      "height": 58
    },
    {
      "id": "cca92735-72c9-427f-95f1-d75593a99cfe",
      "data": {
        "label": "Juego de Unity",
        "themeColor": "default",
        "url": "http://localhost:6543/panal-de-juegos/juego-unity/",
        "icon": null
      },
      "position": {
        "x": -300,
        "y": 3.67394039744206e-14
      },
      "type": "custom",
      "width": 152,
      "height": 58
    }
  ],
  "edges": [
    {
      "id": "edge-e93faa5af2905c0e9c11a8529592f7ae-e883b5546fef432d9ceeb3013b96ebf2",
      "source": "e93faa5a-f290-5c0e-9c11-a8529592f7ae",
      "target": "e883b554-6fef-432d-9cee-b3013b96ebf2",
      "type": "custom-label"
    },
    {
      "id": "edge-e93faa5af2905c0e9c11a8529592f7ae-cca9273572c9427f95f1d75593a99cfe",
      "source": "e93faa5a-f290-5c0e-9c11-a8529592f7ae",
      "target": "cca92735-72c9-427f-95f1-d75593a99cfe",
      "type": "custom-label"
    }
  ]
}
```
### Ejemplo real (200) - default
```json
{
  "id": "7950d98f-015d-5e24-a7d1-cbd115a420e9",
  "title": "Convida Abejas",
  "nodes": [
    {
      "id": "7950d98f-015d-5e24-a7d1-cbd115a420e9",
      "data": {
        "label": "Convida Abejas",
        "themeColor": "root",
        "url": "http://localhost:6543/default/",
        "icon": null
      },
      "position": {
        "x": 0,
        "y": 0
      },
      "type": "custom",
      "width": 200,
      "height": 80
    },
    {
      "id": "3af7a549-2590-4d10-a686-82659346624b",
      "data": {
        "label": "Introduction",
        "themeColor": "default",
        "url": "http://localhost:6543/default/intro/",
        "icon": null
      },
      "position": {
        "x": 300,
        "y": 0
      },
      "type": "custom",
      "width": 152,
      "height": 58
    },
    {
      "id": "df574d76-e720-4d2c-b84b-ab9cfff5c813",
      "data": {
        "label": "Bee Logo",
        "themeColor": "default",
        "url": "http://localhost:6543/default/logo/",
        "icon": "🐝"
      },
      "position": {
        "x": 1.83697019872103e-14,
        "y": 300
      },
      "type": "custom",
      "width": 152,
      "height": 58
    },
    {
      "id": "011b7013-959b-42c9-9cda-adfcef5703bc",
      "data": {
        "label": "Website",
        "themeColor": "default",
        "url": "http://localhost:6543/default/link/",
        "icon": null
      },
      "position": {
        "x": -300,
        "y": 3.67394039744206e-14
      },
      "type": "custom",
      "width": 152,
      "height": 58
    },
    {
      "id": "05181539-f5ef-42f4-b09f-2d804a73d480",
      "data": {
        "label": "Bee Dance",
        "themeColor": "default",
        "url": "http://localhost:6543/default/bee-dance/",
        "icon": "🐝"
      },
      "position": {
        "x": -5.51091059616309e-14,
        "y": -300
      },
      "type": "custom",
      "width": 152,
      "height": 58
    }
  ],
  "edges": [
    {
      "id": "edge-7950d98f015d5e24a7d1cbd115a420e9-3af7a54925904d10a68682659346624b",
      "source": "7950d98f-015d-5e24-a7d1-cbd115a420e9",
      "target": "3af7a549-2590-4d10-a686-82659346624b",
      "type": "custom-label"
    },
    {
      "id": "edge-7950d98f015d5e24a7d1cbd115a420e9-df574d76e7204d2cb84bab9cfff5c813",
      "source": "7950d98f-015d-5e24-a7d1-cbd115a420e9",
      "target": "df574d76-e720-4d2c-b84b-ab9cfff5c813",
      "type": "custom-label"
    },
    {
      "id": "edge-7950d98f015d5e24a7d1cbd115a420e9-011b7013959b42c99cdaadfcef5703bc",
      "source": "7950d98f-015d-5e24-a7d1-cbd115a420e9",
      "target": "011b7013-959b-42c9-9cda-adfcef5703bc",
      "type": "custom-label"
    },
    {
      "id": "edge-7950d98f015d5e24a7d1cbd115a420e9-05181539f5ef42f4b09f2d804a73d480",
      "source": "7950d98f-015d-5e24-a7d1-cbd115a420e9",
      "target": "05181539-f5ef-42f4-b09f-2d804a73d480",
      "type": "custom-label"
    }
  ]
}
```

---

## Visualización de información de nodos individuales
El endpoint <kbd>@resource(path='/api/v1/node/{node_id}', ...)</kbd> define un recurso REST para consultar información detallada de un nodo específico en la estructura de tu aplicación.
### Descripción del funcionamiento
* Permite obtener los datos de un nodo usando su identificador único (node_id).
* Devuelve información como el id, título, contenido, URL, icono, nodos hijos y las conexiones (edges) asociadas a ese nodo.

* Cuando se recibe una petición GET a /api/v1/node/{node_id}, busca el nodo en la estructura interna (root.__nodes__).
* Si el nodo existe, construye un diccionario con sus datos y, si tiene hijos o edges, los incluye en la respuesta.
* Si el nodo no existe, responde con un error 404.
* La respuesta es un JSON con toda la información relevante del nodo, útil para visualizaciones o navegación en la interfaz.

### Ejemplo de uso
- Una vez que se borró previamente la **Data.fs** podemos ver en terminal los id de los contenidos almacenados en la base de datos, entonces ya podemos acceder a su información individual a partir de ese id, de este modo. 

	<kbd>http://localhost:6543/api/v1/node/63c38b62-1450-4ee2-9879-99ed66f04515</kbd>
  
  Se podrá observar en formato **JSON** la información individual del nodo, de esta manera:
  ```json
  {
    "id": "63c38b62-1450-4ee2-9879-99ed66f04515",
    "label": "Bee Dance",
    "contents": "",
    "url": "http://localhost:6543/default/bee-dance/",
    "iconUrl": "🐝",
    "nodes": [],
    "edges": []
  }
  ```


---
## Comunicación entre API y Editor
### 1. **Frontend (React con React Flow)**
- 	El archivo **DiagramCanvas.tsx** es el encargado de mostrar el lienzo del diagrama interactivo.
- 	Cuando el componente se monta (useEffect), hace una petición HTTP al backend:
  ```typescript
	fetch('http://localhost:6543/api/v1/honeycombs/default')
  .then((res) => res.json())
  .then((json) => {
    if (json.nodes && json.edges) {
      setNodes(json.nodes);
      setEdges(json.edges);
    }
  })
  ```
-   Esa llamada espera recibir un JSON con la estructura de nodos y edges (conexiones).
- Una vez recibido, los datos se guardan en el estado (setNodes, setEdges) y React Flow los dibuja en pantalla.
- Si el JSON no contiene el formato esperado, muestra una alerta para el usuario.

---
### 2. Backend (Python con Cornice y Pyramid)
  - El archivo **api.py** define el recurso <kbd>/api/v1/honeycombs/{name}</kbd>.
  - Cuando el frontend pide GET <kbd>/api/v1/honeycombs/default</kbd> , el backend:
  1. Busca el honeycomb solicitado en el árbol de recursos (self.request.root[...]).
  2. Crea un nodo raíz (representa el honeycomb principal).
  3. Genera nodos hijos distribuidos en círculo (cada uno es un "cell").
  - Calcula posiciones (x, y) con trigonometría.
  - Asigna título, ícono y enlace a cada nodo.

4. Crea edges (aristas) desde el nodo raíz hacia cada hijo.
5. Devuelve un JSON con esta estructura:
```json
{
  "id": "uuid-del-root",
  "title": "Título del Honeycomb",
  "nodes": [
    { "id": "...", "data": {...}, "position": {...} },
    { "id": "...", "data": {...}, "position": {...} }
  ],
  "edges": [
    { "id": "edge-...", "source": "...", "target": "...", "type": "custom-label" }
  ]
}
```
---
### 3. Flujo de comunicación
1. El frontend envía una petición GET → /api/v1/honeycombs/default.
2. El backend procesa la solicitud, genera nodos y edges.
4. El backend responde con un JSON → que describe el grafo.
5. El frontend recibe el JSON y lo dibuja con React Flow.

---
## Obteniendo la información del usuario en la sesión
Endpoint <kbd>@resource(path='/api/v1/drones/{userid}', ...)</kbd>
- **Descripción**: Devuelve información del usuario autenticado siempre que el userid en la URL 	coincida con el userid de la identidad autenticada en la petición.
### Autenticación y autorización
- **Autenticación**: Se usa `request.identity` (provista por la configuración de autenticación de la app). Si `request.identity` es false, la respuesta es 401 Unauthorized.
- **Autorización**: Si `request.matchdict['userid'] != request.identity['userid']`, la respuesta es 403 Forbidden (mismatch de userid). Solo el propio usuario puede consultar su información.
### Comportamiento y lógica
1. Obtiene `user = getattr(self.request, 'identity', None)`.
2. Obtiene `url_userid = self.request.matchdict.get('userid')`.
3. Si user es `None` -> **responde 401** con JSON de error.
4. Si `url_userid != user.get('userid')` -> **responde 403** con JSON de error.
5. Si pasa las comprobaciones -> **responde 200** con un objeto JSON con campos tomados de user.
### Ejemplo real
#### ¿Qué pasa si no hizo login el usuario?
![image.png](/image.png)
> 401 Unauthorized — no hay identidad en la petición.
{.is-danger}
#### Se necesita hacer login antes de querer obtener la información del usuario
- Basta con visitar `/login` para que la sesión del usuario se active

![login.png](/login.png)
- Después ya podemos visitar <kbd>@resource(path='/api/v1/drones/{userid}', ...)</kbd>, tomando en cuenta que el id del usuario que está cargado de momento es **convida@unam.social**, entonces el endpoint a visitar es el siguiente <kbd>http://localhost:6543/api/v1/drones/convida@unam.social</kbd> y ahora podremos visualizar la información del usuario en formato JSON

![userid.png](/userid.png)
> 200 OK — petición válida y userid coincide.
{.is-success}

#### Ahora, ¿qué pasa si se digita mal el id del usuario?
Intentaremos poner como id del usuario el siguiente convida@unam.
Recibimos esta respuesta:
![userid2.png](/userid2.png)
> 403 Forbidden — identidad presente pero userid en URL no coincide
{.is-danger}
---
## Recuperación de datos asociados por usuario y nodo
Endpoint <kbd>@resource(path='/api/v1/sipping/{nodeid}', ...)</kbd>
- **Descripción**:  Almacena y recupera datos asociados por usuario y nodo (simulación en memoria). Útil para guardar interacciones previas, estadísticas y logros por usuario para un nodo concreto.
- **Métodos**: 
	- `GET`: Recupera los datos para el par userid:nodeid.
  - `POST`: Guarda/reemplaza los datos para el par userid:nodeid con el JSON enviado.

Para hacer las pruebas necesarias con este endpoint, es necesario hacer login, haciendo uso de este endpoint: <kbd>http://localhost:6543/login</kbd>, esto establecerá los tokens tanto de autenticación `(auth_tkt)` y csrf `(csrf_token)`.
Para poder guardarlos automáticamente en un .txt, llamado `cookies.txt` se puede usar el siguiente comando en terminal `curl.exe -v -c cookies.txt "http://localhost:6543/login"`. Entonces, desde terminal se pueden hacer las pruebas.
### Comportamiento
**GET**
- Busca sipping_data_store[key]
- Si no existe, devuelve objeto por defecto:
```json
{
"interacciones_previas": [],
"estadisticas": {},
"logros": []
}
```
- Devuelve un **200** con ese objeto

**POST**
- Intenta leer `request.json_body`
- Si JSON inválido -> responde **400** y 
```json
{ "error": "Invalid JSON" }
```
- Guarda `sipping_data_store[key]` = payload (sobrescribe si ya existía)
- Responde **200** con 
```json
{ "status": "ok", "saved": payload }
```
### Ejemplo de uso
Podemos crear un archivo .json con la simulación de la estructura esperada y con información cargada, esto para poder mandarlo en el POST. El archivo debe verse de la siguiente manera, cabe mencionar que fue nombrado `payload.json`:
```json
{
  "interacciones_previas": [
    {
      "t": "2026-01-03",
      "score": 10
    }
  ],
  "estadisticas": {
    "visits": 1
  },
  "logros": [
    "inicio"
  ]
}
```
posteriormente podemos usar el siguiente comando en terminal, para mandar la información mediante **POST**:
> 	curl.exe -i -X POST "http://localhost:6543/api/v1/sipping/node-24542b5b-ece1-45fa-975e-6c54744e" `
>    -H "Content-Type: application/json" `
>    -H "X-CSRF-Token: 08e8cf80010e455c8a294c56ff5aa446" `
>    -b cookies.txt `
>    --data @C:\Users\compa\OneDrive\Escritorio\CONVIDA\honeycomb\payload.json
{.is-info}

En donde se especifíca el id del nodo al que simularemos cargarle la información, que en este caso es el nodo con el id `node-24542b5b-ece1-45fa-975e-6c54744e` y necesitamos especificar el **csrf** de la sesión que en este caso es **08e8cf80010e455c8a294c56ff5aa446** y la cookie de la sesión será incrustada directamente del archivo `cookies.txt` y finalmente se manda la información que se obtiene del archivo `payload.json` creado anteriormente.
En la terminal obtendremos una respuesta que se tiene que ver similar a esta:
>  HTTP/1.1 200 OK
>  Content-Length: 141
>  Content-Type: application/json
>  Date: Sun, 04 Jan 2026 01:41:56 GMT
>  Server: waitress
>  X-Content-Type-Options: nosniff
>  
>  {"status": "ok", "saved": {"interacciones_previas": [{"t": "2026-01-03", "score": 10}], "estadisticas": {"visits": 1}, "logros": ["inicio"]}}
{.is-success}


Indicando que la información fue enviada correctamente.
Si acaso hubiera un error en la terminal se vería de esta manera
> HTTP/1.1 400 Bad Request
> Content-Length: 25
> Content-Type: application/json
> Date: Sun, 04 Jan 2026 01:52:04 GMT
> Server: waitress
> X-Content-Type-Options: nosniff
> 
> {"error": "Invalid JSON"}
{.is-danger}

Ahora para verificar la información cargada a dicho nodo mediante **GET**, podemos hacerlo desde el navegador, visitando la siguiente ruta: <kbd>http://localhost:6543/api/v1/sipping/node-24542b5b-ece1-45fa-975e-6c54744e</kbd>, ahí veremos la información previamente enviada el en **POST**. Veremos algo así.

![nodos.png](/nodos.png)

---
## Obtener id de usuario que hizo login
Endpoint <kbd>@resource(path='/api/v1/userid', ...)</kbd>
- **Descripción**: corresponde a un recurso de la API que permite obtener el identificador único (userid) del usuario autenticado, accediendo nuevamente a <kbd>http://localhost:6543/login</kbd>.
### Comportamiento
1. Autenticación:
	- Se obtiene el usuario autenticado desde `self.request.identity`.
	- Si no hay un usuario autenticado (user es None), responde con un código de estado **401** 		Unauthorized y un mensaje de error.
  2. Respuesta:
 	- Si el usuario está autenticado, devuelve un JSON con el userid del usuario.
 
### Ejemplo real
Una vez que hicimos login mediante <kbd>http://localhost:6543/login</kbd>, podemos obtener el userid del usuario mediante <kbd>http://localhost:6543/api/v1/userid</kbd>, si el login fue exitoso y hay un usuario con una sesión activa veremos una respuesta como la siguiente
```json
{"userid": "convida@unam.social"}
```
En caso de que no haya sesión activa, es decir que no se haya hecho login, obtendremos el siguiente mensaje 
```json
{"error": "Unauthorized"}
```

## Diseño inicial

> La información del proceso de conexión de la API con el Front-end se encuentra en la siguiente página: [Conexión cliente servidor](/componentes/API/Conexión_Cliente_Servidor).
{.is-info}


Obtención de contenidos desde la Base de Datos Orientada a Objetos hacia el cliente (FrontEnd).
> ![contenidosinteractivoshaciafront.png](/contenidosinteractivoshaciafront.png)
Diagrama general de la comunicación desde la BD orientada a objetos con el cliente

Este diagrama muestra cómo funciona el flujo de obtención de contenidos interactivos a través de una API desarrollada con Cornice y Pyramid, y cómo se conecta con la interfaz Swagger para documentación. A continuación se explicará paso a paso:

**1. El Cliente inicia la solicitud**
Todo comienza cuando el cliente (por ejemplo, una aplicación web o móvil) realiza una solicitud HTTP del tipo GET /contenidos. Esta petición busca obtener contenido interactivo desde el servidor.

**2. Cornice (Servicio)**
La solicitud llega a Cornice, que es un componente de la arquitectura Pyramid para definir servicios web.
Aquí se recibe y valida la solicitud. Cornice actúa como una capa intermedia entre el cliente y la lógica de negocio.
Además, Cornice también expone una ruta especial: **__api/__o__/__services**, la cual es utilizada por Swagger UI para generar automáticamente la documentación de los endpoints.

**3. Swagger UI (Documentación de la API)**
Gracias a esa ruta especial de Cornice, Swagger puede generar una interfaz gráfica donde se visualiza toda la documentación de la API. Desde ahí, los desarrolladores pueden probar los endpoints, ver qué parámetros se necesitan y qué respuesta se espera.

**4. Vista (Handler API)**
Luego, Cornice llama a una función decorada con @Service. Esa función actúa como un handler o controlador, donde se ejecuta la lógica real para obtener los datos. Esta parte se conoce como la Vista o handler de la API.

**5. Acceso a la base de datos**
Dentro de esa función, se hace una llamada interna en Python (por ejemplo root.get(...)) que accede directamente a la base de datos. Se recupera el contenido solicitado, ya sea un recurso interactivo, datos estáticos o dinámicos.

**6. Estructura lista para JSON**
Una vez recuperado el contenido, el handler construye un objeto estructurado que Cornice y Pyramid pueden convertir fácilmente en formato JSON.

**7. Respuesta al Cliente**
Finalmente, Cornice devuelve una respuesta HTTP 200 OK con el contenido convertido a JSON.
Este JSON contiene todos los datos del contenido solicitado, listo para ser usado en el front-end del cliente.
