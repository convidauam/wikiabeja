---
title: Paisaje Temático
description: El componente de paisaje temático se encarga de visualizar y nevegar los cobntenidos de un convida. 
published: true
date: 2026-02-11T00:36:04.320Z
tags: componente, navegación
editor: markdown
dateCreated: 2026-02-11T00:36:04.320Z
---

# Paisaje temático


Representa paisajes temáticos* navegables por el usuario, representando grafos como "mapas" en donde existen nodos tipo nivel, nodos conectores y conexiones entre nodos.

Los mapas presentan una estética inspirada en los mapas, sistemas de navegación y mapas de habilidades presentes en videojuegos.

Se desarrolla actualmente en https://github.com/AstroAmoeba/FlightpathPixi

![FlightpathDemo-ezgif com-optimize](https://github.com/user-attachments/assets/f4682be2-8059-4814-8572-432ac6e9d51c)

---

## Features

- **Representación**: Se representan nodos en forma de territorios, cada nodo tipo nivel tiene una página asociada mediante la cual se puede acceder a un contenido.*
  
- **Niveles o contenidos educativos**: En la carpeta `public/pages/levels/` hay varios niveles o secciones temáticas sobre distintos aspectos del mundo de las abejas (organización, reproductivo, vuelo, diversidad, agricultura, etc.).
- **[A implementar] Soporte para rutas múltiples y aprendizaje progresivo**: El usuario puede navegar entre los distintos temas/niveles siguiendo rutas generadas a partir de parámetros dados, de manera que la navegación entre contenidos sea personalizada. Existen elementos de progresión en las rutas.

---

## Estructura General 
(a detallar)

```
public/
  pages/
    ...
src/
  hooks/
    useCharacterMovement.ts    ← Lógica de movimiento de personajes en el “mapa” (ver detalles abajo)
  types/
    WorldTypes.ts              ← Tipado y estructura de “mundos” y nodos del mapa interactivo
README.md
```

### Desglose de archivos clave y funcionalidad

- `src/hooks/useCharacterMovement.ts`  
  Implementa la animación y lógica de movimiento del personaje principal sobre el mapa interactivo.  
  - Usa React hooks y controla el sprite del personaje y animaciones hacia los distintos nodos del mundo.  
  - Fragmento relevante de código:
    ```typescript name=src/hooks/useCharacterMovement.ts url=https://github.com/AstroAmoeba/FlightpathPixi/blob/256970b3d79e9af2b4f7c91263de6cacc342dc38/src/hooks/useCharacterMovement.ts#L41-L70
    // Animación suave del personaje hacia el nodo objetivo
    useEffect(() => {
      if (!mapRenderer || worlds.length === 0) return;
      const world = worlds[currentWorld];
      const currentNode = world.nodes.find(n => n.id === currentNodeId);
      if (!currentNode) return;

      const character = mapRenderer.getCharacter();
      if (!character || !character.position) return;

      const targetX = currentNode.x;
      const targetY = currentNode.y;

      const distance = Math.sqrt(
        Math.pow(character.position.x - targetX, 2) + 
        Math.pow(character.position.y - targetY, 2)
      );

      if (distance < 1) {
        return;
      }
      // ...
    ```
- `src/types/WorldTypes.ts`  
  Define las estructuras y tipos principales del “mapa de mundos”, incluyendo nodos, conexiones y datos del nivel:
    ```typescript name=src/types/WorldTypes.ts url=https://github.com/AstroAmoeba/FlightpathPixi/blob/256970b3d79e9af2b4f7c91263de6cacc342dc38/src/types/WorldTypes.ts#L1-L37
    export interface NodeData {
      id: número;
      gridX: número;
      gridY: número;
      title: string;
      url: string;
      type: 'level' | 'connector';
      connections: {
        up?: number;
        down?: number;
        left?: number;
        right?: number;
      };
    }
    // ...
    export interface World {
      id: number;
      name: string;
      nodes: Node[];
      startNodeId: number;
    }
    ```

- `public/pages/`  
  Carpeta con todas las páginas educativas y visuales del proyecto:
  - `bee-anatomy.html`, `bee-hive.html`, `bee-communication.html`, `bee-conservation.html`, `honey-production.html`, `pollination.html`:  
    → Páginas educativas HTML sobre la biología y ecología de las abejas. Cada archivo se enfoca en un aspecto diferente, incluyendo explicaciones, ilustraciones y listas temáticas.
  - `levels/`  
    Subcarpeta con archivos HTML para cada “nivel” o categoría del tema de abejas, incluyendo roles de las abejas (obrera, nodriza, guardiana, forrajera, etc.), procesos (reproducción, polinización, agricultura, etc.) y aspectos biológicos y sociales.

---

##  Funciones 

- **Animación y lógica de movimiento de personajes:**  
  - Archivo: [`src/hooks/useCharacterMovement.ts`](https://github.com/AstroAmoeba/FlightpathPixi/blob/256970b3d79e9af2b4f7c91263de6cacc342dc38/src/hooks/useCharacterMovement.ts)  
  - Ubicación: Hook `useCharacterMovement`.  
  - Rol: Moviliza al personaje en el mapa de niveles, siguiendo el sistema de nodos.

- **Definición de estructura de mapa y nodos:**  
  - Archivo: [`src/types/WorldTypes.ts`](https://github.com/AstroAmoeba/FlightpathPixi/blob/256970b3d79e9af2b4f7c91263de6cacc342dc38/src/types/WorldTypes.ts)
  - Rol: Define los tipos, nodos, mundos y conexiones de navegación.

 - **Contenidos**  
  - Ubicación:  
    - `public/pages/*.html`: contenido temático de cada sección o nivel.
    - `public/pages/levels/*.html`: niveles o categorías sobre el trabajo, roles y biología de las abejas.

###  Archivos JSON en `/data`

En la carpeta `/data` se encuentran archivos en formato JSON que cumplen funciones esenciales para la estructura y la dinámica del repositorio:

- **Definición de mapas y niveles:** Los archivos JSON contienen la información estructural de los diferentes "mundos", mapas o rutas navegables, describiendo nodos, conexiones y propiedades que permiten ensamblar dinámicamente los paisajes educativos.

- **Soporte para contenido dinámico:** La aplicación lee estos archivos para mostrar rutas, nodos, títulos y referencias a páginas de contenido, facilitando la actualización o creación de nuevos recorridos y temáticas sin modificar el código fuente de la lógica principal.

## Responsables

Sonia Pérez
