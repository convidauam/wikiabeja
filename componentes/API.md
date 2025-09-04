---
title: API
description: API CONVIDA
published: true
date: 2025-09-04T19:31:36.234Z
tags: api
editor: markdown
dateCreated: 2025-07-22T01:01:03.085Z
---

# API
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

## API honeycomb
La API Honeycomb permite gestionar y visualizar estructuras de honeycombs (panales) representadas como grafos con nodos y aristas. Cada honeycomb es un nodo raíz con celdas (nodos hijos) distribuidas de manera circular, conectadas mediante aristas.
### ¿Qué tecnologías se ocuparon?
* **Python 3.x**
Lenguaje base.

* **Pyramid**
 Framework web principal para crear la aplicación WSGI.

* **Cornice**
 Extensión para Pyramid que facilita la creación de APIs REST.

* **Cornice-Swagger**
 Genera automáticamente documentación OpenAPI/Swagger a partir de los recursos y docstrings de Cornice.

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
```
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
```
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
```
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
## Comunicación entre API y Editor


