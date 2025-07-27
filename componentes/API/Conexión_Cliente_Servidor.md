---
title: Conexión del Cliente con Servidor
description: Conexión de la API (back-end) con el cliente (front-end)
published: true
date: 2025-07-27T04:47:13.030Z
tags: api
editor: markdown
dateCreated: 2025-07-22T01:03:40.890Z
---

# Conexión del Back-end con el Front-end

El proceso de comunicación entre el Front-end CONVIDA y el Back-end CONVIDA se inicia con una fase crucial de descubrimiento y definición. El Front-end CONVIDA comienza realizando una lectura del archivo `openapi.json`, el cual es expuesto directamente por el back-end. Como se ilustra en el _Diagrama 1_, el back-end hace accesible este archivo a través de la URL `http://<ip>:<puerto>/openapi/openapi.json`.

Este archivo `openapi.json` es el corazón de la interacción, ya que funge como una descripción completa y estandarizada de la API. En él se detalla minuciosamente la estructura de la API, incluyendo la enumeración de los *endpoints* disponibles, los métodos HTTP permitidos para cada uno (como GET, POST, PUT, DELETE), los parámetros esperados para las solicitudes y los formatos de las respuestas. Para la generación y visualización de esta documentación OpenAPI en el Back-end, se utiliza [Swagger](https://swagger.io/), específicamente a través de la extensión de Cornice, [Cornice Swagger](https://cornices.github.io/cornice.ext.swagger/). Esta herramienta facilita no solo la exposición de la API, sino también su modificación y comprensión por parte de los desarrolladores.

Una vez que el Front-end ha procesado el archivo `openapi.json`, procede a la definición de métodos. Esto implica que el Front-end adquiere un conocimiento preciso de cómo interactuar con la API del Back-end. Para este propósito, el Front-end emplea [Axios](https://axios-http.com/es/), una librería de JavaScript ampliamente reconocida para realizar solicitudes HTTP. En particular, se utiliza [openapi-client-axios](https://openapistack.co/docs/openapi-client-axios/intro/), que aprovecha la definición del esquema OpenAPI para generar un cliente HTTP robusto y tipado. Este cliente permite al Front-end enviar peticiones de manera estructurada y segura. La invocación a los métodos del cliente se realiza utilizando los nombres definidos en el campo `operationId` dentro del esquema OpenAPI, expuesto por el Back-end.

Con los métodos debidamente definidos y el Front-end equipado para la comunicación, el cliente puede proceder a enviar peticiones al servidor utilizando métodos sencillos y auto-documentados del cliente generado.

Finalmente, el Back-end procesa cada una de estas peticiones y, en consecuencia, envía las respuestas correspondientes de vuelta al Front-end. Estas respuestas pueden contener desde los datos solicitados, confirmaciones de acciones realizadas, hasta mensajes de error detallados, completando así un ciclo de comunicación bidireccional eficiente y bien definido entre el cliente y el servidor.

![axios-openapi.png](/axios-openapi.png){.align-center}
- Diagrama 1. Definición y conexión del cliente con el servidor

## Diagrama de Casos de Uso

![caso_de_uso_openapi.png](/caso_de_uso_openapi.png){.align-center}
- Diagrama 2. Casos de uso de la definición y conexión del cliente con el servidor

## Ejemplo de uso

Para ilustrar este proceso, consideremos un ejemplo tanto en el Back-end como en el Front-end.

### En el Back-end (usando Cornice Swagger)
Con Cornice Swagger correctamente configurado, es esencial asignar un operationId a los diferentes endpoints. A continuación, se muestra un ejemplo:
```python 
@my_service.get(operation_id='getExample')
def get_example(request):
    """Obtiene un ejemplo."""
    return {"msg": "ok", "data": "data"}
```

En este caso, hemos definido un endpoint GET con el operationId `getExample`, que servirá para obtener datos de ejemplo.

### En el Front-end (usando openapi-client-axios)
En el Front-end, la creación del cliente y la comunicación con sus métodos se puede realizar de la siguiente forma:

```js 
const OpenAPIClientAxios = require('openapi-client-axios').default;

const api = new OpenAPIClientAxios({
  definition: 'http://<>:<>/openapi/openapi.json',
  axiosConfigDefaults: { baseURL: 'http://<>:<>' }
});

async function createExamples() {
  const client = await api.init();
  let result = await client.getExample();
  console.log('GET result:', result.data);
}
```

Como se observa, una vez inicializado el cliente `api`, podemos llamar directamente al método `client.getExample()`, el cual corresponde al `operationId` definido en el Back-end, facilitando una interacción directa y auto-documentada.

## Responsable
* [Jorge Júarez (Yorch)](https://github.com/jorge-jrzz)
