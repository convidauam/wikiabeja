---
title: Creación del mapa
description: En este apartado se describe de manera corta el procedimiento que se siguió para crear el mapa del contenido Defiende la Colmena
published: true
date: 2025-09-30T01:09:17.306Z
tags: 
editor: markdown
dateCreated: 2025-09-30T01:09:17.306Z
---

# Mapa
Para el mapa se optó por crear un mapa **isométrico**. *Isométrico* se refiere a una técnica de ilustración 2D que intenta aparentar estructuras 3D a través del ángulo o la perspectiva que se utiliza para presentar la imagen al observador.  
En la **Imagen 1** podemos observar un ejemplo de un mapa isométrico. Es importante notar que todo está creado en 2D, con la ilusión de tener componentes con altura, anchura y profundidad.

![ejemplomapaisometrico.png](/ejemplomapaisometrico.png){.align-center}
<p align="center" style="font-size: 14px;">
  Imagen 1. Ejemplo de mapa isométrico
</p>

Para poder crear un mapa isométrico es necesario seguir un par de pasos. Todos estos pasos fueron obtenidos del blog oficial [Entornos isométricos 2D con Tilemap](https://unity.com/es/blog/engine-platform/isometric-2d-environments-with-tilemap).  
Si se requiere más detalle, se recomienda consultar el artículo completo.

---

## Configuración del proyecto
Para poder representar correctamente los ejes del mundo dentro del proyecto, debemos realizar la siguiente modificación:

1. Ingresar a la pestaña ***Editar > Configuración del proyecto > Gráficos***.  
2. Una vez ahí, modificar los valores de transparencia de los ejes **X**, **Y** y **Z**.

De manera predeterminada, estos valores están en `(0, 0, 1)`, pero debemos cambiarlos a `(0, 1, 0)`, como se muestra en la **Imagen 2**.

![configuracionejes.png](/configuracionejes.png){.align-center}
<p align="center" style="font-size: 14px;">
  Imagen 2. Modificación de la transparencia de los ejes dentro de la configuración del proyecto
</p>

---

## Creación de los *Tilesets*
Un mapa isométrico está compuesto por ***tiles*** o ***baldosas***. Estos *tiles* permiten crear infinidad de mapas utilizando una única base de imágenes.  
Un ***tileset*** no es más que un conjunto de *tiles*.

El *tileset* utilizado para este contenido fue creado con ayuda de la herramienta **Inkscape**. Este *tileset* se puede observar en la **Imagen 3**.

![tileset.png](/tileset.png){.align-center}
<p align="center" style="font-size: 14px;">
  Imagen 3. Tileset creado para el mapa
</p>

Como podemos observar, se crearon *tiles* básicos que representan zonas de pasto, agua y tierra. De igual manera, se diseñaron *tiles* que dan la ilusión de mayor altura, así como elementos decorativos.

---

## Creación del mapa
Para comenzar a construir el mapa, primero debemos crear un objeto:  
***Objeto 2D > Tilemap > Isométrico***, como se muestra en la **Imagen 4**. De esta manera, generaremos un *lienzo* sobre el cual podremos construir nuestro mapa.

![creaciontilemap.png](/creaciontilemap.png){.align-center}
<p align="center" style="font-size: 14px;">
  Imagen 4. Creación de un Tilemap dentro de Unity
</p>

Posteriormente, debemos agregar nuestro *tileset* a los **assets** de Unity para poder utilizarlo.  
Una vez importado, abrimos la herramienta ***Tile Palette*** y arrastramos todos los *assets* correspondientes a nuestro *tileset* dentro de esta herramienta, lo cual nos permitirá visualizar lo que se muestra en la **Imagen 5**.

![herramientatilepalette.png](/herramientatilepalette.png){.align-center}
<p align="center" style="font-size: 14px;">
  Imagen 5. Visualización de nuestros tiles dentro de la herramienta Tile Palette
</p>

Una vez que tenemos nuestra paleta configurada, ya podemos comenzar a ***dibujar*** sobre nuestro **Tilemap**.  
Esto se puede hacer de manera libre, sin preocuparnos por el tamaño o la estructura del mapa. Para el **Nivel 1**, se diseñó el mapa mostrado en la **Imagen 6**.

![mapa.png](/mapa.png){.align-center}
<p align="center" style="font-size: 14px;">
  Imagen 6. Mapa del contenido interactivo
</p>
