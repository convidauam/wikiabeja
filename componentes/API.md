---
title: API
description: API CONVIDA
published: true
date: 2025-07-25T02:24:02.658Z
tags: api
editor: markdown
dateCreated: 2025-07-22T01:01:03.085Z
---

# API
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