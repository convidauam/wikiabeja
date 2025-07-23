---
title: Diagrama General
description: En este apartado se muestra el diagrama general del contenido interactivo "Defiende la colmena"
published: true
date: 2025-07-23T17:35:43.765Z
tags: 
editor: markdown
dateCreated: 2025-07-21T20:02:05.660Z
---

A continuacion se muestra el diagrama que ejemplifica la interacción entre el contenido interactivo *Defiende la colmena* y la plataforma Convida.
![diagrama_general_2.0.png](/diagrama_general_2.0.png "Text under the image"){.align-center}
<p align="center" style="font-size: 14px;">
  Imagen 1. Diagrama General de "Defiende la colmena"
</p>



Para tener un mejor entendimiento del diagrama, este será explicado paso por paso:
1. Como primer paso el usuario debe ingresar a la plataforma Convida Abejas para poder ingresar a sus distintos módulos.
2. Una vez que estemos dentro de la plataforma se debe navegar al contenido interactivo *Defiende la colmena* y seleccionarlo, esto hará que la plataforma despliegue el contenido, permitiendo al usuario jugar y avanzar en la experiencia.
3. Una vez que el usuario deja de jugar y sale del contenido interactivo, las métricas o datos de guardado son enviados a la API convida mediante archivos JSON.
4. La API convida se comunica con el Backend de la aplicacion para poder mandar las métricas del usuario a guardar.
5. Finalmente, el Backend se encarga de guardar la información en la base de datos.
