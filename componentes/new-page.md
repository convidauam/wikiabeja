---
title: Gestión de nodos
description: 
published: true
date: 2026-01-06T02:15:59.467Z
tags: 
editor: markdown
dateCreated: 2026-01-06T01:52:07.514Z
---

# Gestión de nodos en beehive
## Añadir aristas de un nodo al índice global
`_add_node_edges`
- **Descripción**: 
añade (**append**) las aristas de un nodo al índice global `__edges__`. Recorre recursivamente los hijos (atributo `nodes`) y, si el nodo es un `PersistentMapping`, recorre sus valores para agregar sus aristas también.
- **Parámetros**:
`node`; cualquier objeto que pueda tener `.edges` y/o `.nodes` o ser un `PersistentMapping`.
- **Funcionamiento**:
determina `node_id` (por `__name__` o `id`); si `node.edges` existe y es lista, asegura que `self.__edges__[node_id]` exista y hace `.append(edge)` para cada arista; luego desciende a `node.nodes` y/o valores del `PersistentMapping` recursivamente llamándose a sí misma para los hijos.

---
## Sincronizar aristas de un objeto tipo grafo
`sync_graph_edges`
- **Descripción**: 
sincroniza las aristas de un objeto tipo grafo (`HoneycombGraph` u objeto con `.edges`) con el índice global `__edges__`, reemplazando las aristas previas del grafo por las actuales.
- **Parámetros**: 
`graph_node`; objeto que representa el grafo; se usa `graph_node.__name__` o `str(graph_node.id)` como clave.
- **Funcionamiento**: 
calcula `node_id`, elimina cualquier entrada previa `self.__edges__[node_id]` y, si `graph_node` tiene .edges (lista o `PersistentList`), crea una nueva `PersistentList` en `self.__edges__[node_id]` y copia las aristas actuales.

---
## Eliminar un nodo del índice global de nodos
- **Descripción**: 
elimina un nodo del índice global de nodos `__nodes__`, todos sus descendientes (si existen) y todas las aristas (`__edges__`) relacionadas, tanto las que tienen al nodo/descendiente como origen como las que lo tienen como destino.
- **Parámetros**:
`node_id`; clave usada en `__nodes__` (normalmente `node.__name__` o `str(node.id))`.
- **Funcionamiento**: 
busca el nodo en `__nodes__`; si existe, hace un recorrido recursivo recogiendo los identificadores de ese nodo y sus hijos (busca atributos `nodes` tipo lista/`PersistentList` o recorre el `PersistentMapping`), borra esos IDs de `__nodes__`, elimina entradas en `__edges__` cuyo origen sea alguno de esos IDs y filtra/elimina aristas en otras entradas cuyo destino sea alguno de esos IDs.

## Test de las funciones
`test_add_and_remove_nodes`
En esta función se pretende realizar un test para probar dicha funcionalidad de agregar nodos y que sus conexiones igualmente se agreguen al índice. La función incluye prints que permiten visualizar en terminal los resultados que vamos obteniendo. Esta es la función para nuestra primera prueba.
```python
def test_add_and_remove_nodes():
    beehive = BeeHive()
    nodo1 = CellText(name="nodo1", contents="contenido1")
    nodo2 = CellText(name="nodo2", contents="contenido2")
    edge = CellEdge(name="edge1", title="conexión", from_node=nodo1, to_node=nodo2)

    graph = HoneycombGraph(name="grafo1")
    graph.add_node(nodo1)
    graph.add_node(nodo2)
    graph.add_edge(edge)

    beehive.add_node(nodo1)
    beehive.add_node(graph)

    print("Antes de eliminar:")
    print("NODOS:", list(beehive.__nodes__.keys()))
    print("EDGES:", {k: [e.title for e in v] for k, v in beehive.__edges__.items()})

    # Elimina solo el hijo (nodo1)
    beehive.remove_node_recursively("nodo1")
    print("\nDespués de eliminar nodo1:")
    print("NODOS:", list(beehive.__nodes__.keys()))
    print("EDGES:", {k: [e.title for e in v] for k, v in beehive.__edges__.items()})
    assert "nodo1" not in beehive.__nodes__
    assert "grafo1" in beehive.__nodes__
    assert "grafo1" in beehive.__edges__

    # Vuelve a agregar nodo1 para probar la eliminación del padre
    beehive.add_node(nodo1)
    print("\nAntes de eliminar grafo1:")
    print("NODOS:", list(beehive.__nodes__.keys()))
    print("EDGES:", {k: [e.title for e in v] for k, v in beehive.__edges__.items()})

    # Elimina el padre (grafo1)
    beehive.remove_node_recursively("grafo1")
    print("\nDespués de eliminar grafo1:")
    print("NODOS:", list(beehive.__nodes__.keys()))
    print("EDGES:", {k: [e.title for e in v] for k, v in beehive.__edges__.items()})
    assert "grafo1" not in beehive.__nodes__
    assert "nodo1" not in beehive.__nodes__
    assert "grafo1" not in beehive.__edges__
```

- **Contexto inicial**:
	- se crea un objeto `BeeHive` y se añaden nodos (`nodo1`, `nodo2`) y un grafo (**grafo1**) que 	contiene estos nodos y una arista entre ellos.
	- Los nodos y el grafo se registran en los índices globales `__nodes__` y `__edges__`.
- **Primera eliminación (`nodo1`)**:
	- Se llama a `remove_node_recursively("nodo1")`.
	- La función elimina `nodo1` de `__nodes__` y elimina cualquier arista en `__edges__` donde `nodo1` sea origen o destino.
	- Resultado:
		- `nodo1` ya no está en `__nodes__`.
		- Las aristas relacionadas con `nodo1` se eliminan de `__edges__`.
- **Segunda eliminación (grafo1)**:
	- Se vuelve a agregar `nodo1` y luego se elimina grafo1 con `remove_node_recursively("grafo1")`.
	- Esto elimina grafo1 y todos sus nodos hijos (`nodo1`, `nodo2`) y sus aristas del índice global.
	- Resultado:
		- grafo1, `nodo1`, y `nodo2` ya no están en `__nodes__`.
		- Todas las aristas relacionadas con grafo1 se eliminan de `__edges__`.
    
---

`test_sync_graph_edges`
Esta función para el test pretende registrar los nodos con sus respectivas aristas, estas aristas se actualizarán según se vayan gestionando los nodos. La función es la siguiente.
```python
def test_sync_graph_edges():
    beehive = BeeHive()
    nodo1 = CellText(name="n1", contents="c1")
    nodo2 = CellText(name="n2", contents="c2")
    edge1 = CellEdge(name="e1", title="A", from_node=nodo1, to_node=nodo2)

    graph = HoneycombGraph(name="g1")
    graph.add_node(nodo1)
    graph.add_node(nodo2)
    graph.add_edge(edge1)

    beehive.add_node(graph)
    # Verifica que la arista está en el índice global
    assert "g1" in beehive.__edges__
    assert any(e.title == "A" for e in beehive.__edges__["g1"])

    print("Antes de sync_graph_edges:", [e.title for e in beehive.__edges__["g1"]])

    # Modifica las aristas internas del grafo
    edge2 = CellEdge(name="e2", title="B", from_node=nodo2, to_node=nodo1)
    graph.edges.append(edge2)

    # Sincroniza el índice global
    beehive.sync_graph_edges(graph)

    print("Después de sync_graph_edges:", [e.title for e in beehive.__edges__["g1"]])

    # Ahora debe contener ambas aristas
    titles = [e.title for e in beehive.__edges__["g1"]]
    assert "A" in titles and "B" in titles
```
- **Contexto inicial**:
	- Se crea un grafo g1 con nodos (n1, n2) y una arista (A).
	- El grafo se añade al `BeeHive`, registrando sus nodos y aristas en los índices globales.
- **Modificación del grafo**:
	- Se añade una nueva arista (B) al grafo g1.
- **Sincronización**:
	- Se llama a `sync_graph_edges(graph)`.
	- La función elimina las aristas previas de g1 en `__edges__` y las reemplaza con las aristas actuales del grafo (A y B).
	- Resultado:
		- `__edges__["g1"]` ahora contiene ambas aristas (A y B).

---
`test_add_node_with_uuid_key`
Este test verifica que el sistema puede manejar nodos sin un `__name__` definido, asignándoles un UUID único y registrando correctamente sus aristas en el índice global. La función que permite probar esto, es la siguiente.
```python
def test_add_node_with_uuid_key():
    beehive = BeeHive()
    # Crear un nodo sin __name__ (o con None)
    nodo1 = CellText(name=None, contents="contenido1")
    nodo2 = CellText(name=None, contents="contenido2")
    edge = CellEdge(name=None, title="conexión-uuid", from_node=nodo1, to_node=nodo2)

    graph = HoneycombGraph(name=None)
    graph.add_node(nodo1)
    graph.add_node(nodo2)
    graph.add_edge(edge)

    beehive.add_node(graph)

    print("\nNODOS EN __nodes__ (UUID):")
    for k, v in beehive.__nodes__.items():
        print(f"  clave: {k}, tipo: {type(v).__name__}")

    print("\nEDGES EN __edges__ (UUID):")
    for k, v in beehive.__edges__.items():
        print(f"  clave: {k}, edges: {[e.title for e in v]}")

    # Verifica que la clave es un UUID (no None ni string vacía)
    assert all(k is not None and str(k) != '' for k in beehive.__nodes__.keys())
    # Verifica que la arista está en el índice de edges
    found = False
    for edges in beehive.__edges__.values():
        if isinstance(edges, PersistentList):
            if any(e.title == "conexión-uuid" for e in edges):
                found = True
    assert found, "La arista con UUID no fue agregada al índice de edges"
```
- **Contexto inicial**:
  - Se crea un objeto `BeeHive`.
  - Se crean dos nodos (`nodo1` y `nodo2`) sin un atributo `__name__` definido (o con None).
  - Se crea una arista (`edge`) que conecta estos nodos.
  - Se crea un grafo (`graph`) que contiene los nodos y la arista.
- **Uso de `add_node` (y `_add_node_edges` indirectamente)**:
  - Se añade el grafo al `BeeHive` con `beehive.add_node(graph)`.
  - Internamente, `_add_node_edges` registra las aristas del grafo en el índice global `__edges__`.
  - Como los nodos no tienen un `__name__`, se les asigna un UUID único como clave en `__nodes__`.
- **Verificaciones**:
  - Se imprime el contenido de `__nodes__` y `__edges__` para verificar que las claves son UUIDs y 		 que las aristas están correctamente registradas.
  - Se asegura que:
  	- Todas las claves en `__nodes__` son UUIDs válidos.
  	- La arista `conexión-uuid` está presente en el índice global `__edges__`.

