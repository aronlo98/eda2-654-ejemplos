# Grafos

Este documento contiene material de apoyo para el tema de **grafos** en el curso de **Estructuras de Datos 2**. Un grafo es una estructura de datos que permite representar **relaciones** entre objetos, por ejemplo conexiones entre personas, ciudades, computadoras o estados de un sistema.

En un grafo:

- los puntos se llaman **vertices**,
- las conexiones se llaman **aristas**.

De manera formal, un grafo puede representarse como un par `G = (V, E)` donde:

- `V` es el conjunto de vertices,
- `E` es el conjunto de aristas que conectan pares de vertices.

## Aplicaciones

Los grafos aparecen en muchos problemas reales, por ejemplo:

- **redes sociales**,
- **mapas y sistemas GPS**,
- **redes de computadoras**,
- **videojuegos**,
- **recomendadores y rutas**.

## Tipos de Grafos

### Grafo no dirigido

En un grafo no dirigido, una arista conecta dos vertices en ambos sentidos.

Ejemplo:

```text
A --- B
```

Aqui, si `A` esta conectado con `B`, entonces `B` tambien esta conectado con `A`.

### Grafo dirigido

En un grafo dirigido, las aristas tienen direccion.

Ejemplo:

```text
A --> B
```

Aqui, la conexion va de `A` hacia `B`, pero no necesariamente de `B` hacia `A`.

## Definiciones Basicas

### Camino

Un **camino** es una secuencia de aristas que permite llegar de un vertice a otro.

### Circuito

Un **circuito** es un camino que empieza y termina en el mismo vertice.

### Ciclo

Un **ciclo** es un circuito en el que no se repiten vertices, excepto el primero y el ultimo.

### Grafo conectado

Un grafo es **conectado** si existe un camino entre cualquier par de vertices.

## Representacion Computacional

Las dos formas mas comunes de representar un grafo en una computadora son:

1. **Matriz de adyacencia**
2. **Lista de adyacencia**

## Matriz de Adyacencia

La matriz de adyacencia usa una tabla de `n x n`, donde `n` es el numero de vertices. En cada posicion se indica si existe o no una arista entre dos vertices.

Su complejidad espacial es:

```text
Theta(n^2)
```

Es una buena opcion cuando el grafo tiene muchas aristas.

### Ejemplo

Supongamos el siguiente grafo no dirigido:

```text
A --- B
|
C
```

La matriz de adyacencia seria:

```text
    A B C
A [ 0 1 1 ]
B [ 1 0 0 ]
C [ 1 0 0 ]
```

## Lista de Adyacencia

La lista de adyacencia guarda, para cada vertice, una lista de sus vecinos.

Su complejidad espacial es:

```text
Theta(n + m)
```

donde:

- `n` es el numero de vertices,
- `m` es el numero de aristas.

Es una mejor opcion cuando el grafo es disperso, es decir, cuando tiene pocas aristas respecto al numero de vertices.

### Ejemplo

Para el mismo grafo:

```text
A --- B
|
C
```

La lista de adyacencia seria:

```text
A = [B, C]
B = [A]
C = [A]
```

## Comparacion Rapida

- La **matriz de adyacencia** permite verificar rapidamente si existe una arista entre dos vertices.
- La **lista de adyacencia** usa menos memoria en grafos dispersos.
- La eleccion depende del problema que queremos resolver y de la densidad del grafo.

## Ejemplo de Modelado

Podemos representar un mapa sencillo de ciudades:

```text
Lima --- Cusco
  |
Arequipa
```

En este caso:

- los vertices son las ciudades,
- las aristas representan rutas o conexiones.

Este tipo de modelado ayuda a resolver problemas como:

- encontrar caminos,
- analizar conectividad,
- optimizar rutas.

## Conclusiones

Los grafos son una estructura fundamental para modelar relaciones entre elementos. Comprender sus conceptos basicos y sus formas de representacion es importante porque sirve de base para algoritmos mas avanzados, como recorridos, caminos minimos y analisis de redes.
