# Grafos

Este documento contiene material de apoyo para el tema de **grafos** en el curso de **Estructuras de Datos 2**. Un grafo es una estructura formada por un conjunto de puntos conectados mediante líneas, muy útil para representar relaciones en aplicaciones reales como **redes sociales**, **mapas y sistemas GPS**, **redes de computadoras** y **videojuegos**.

De manera formal, un grafo `G = (V, E)` es un par ordenado donde `V` es un conjunto no vacío de vértices y `E` es un conjunto de aristas.

## Propiedades y Definiciones

Todo grafo se rige por conceptos básicos que permiten analizarlo:

1. **Vértices y Aristas**: Los puntos se denominan *vértices* y las líneas de conexión *aristas* (o *edges*).
2. **Dirección**: Un grafo puede ser **no dirigido** (las aristas se transitan en ambos sentidos) o **dirigido** (las aristas tienen una dirección definida, como flechas).
3. **Camino (Path)**: Secuencia de aristas que permite llegar desde un vértice *v* hasta un vértice *w* sin repetir aristas.
4. **Circuito (Circuit)**: Un camino que comienza y termina en el mismo vértice.
5. **Ciclo (Cycle)**: Un circuito en el que ningún vértice se repite, excepto el primero y el último.
6. **Conectividad**: Un grafo está conectado si existe un camino entre cualquier par de vértices.

## Idea General de Representación

Para representar computacionalmente un grafo existen diversas formas. Las más comunes son:

1. **Matriz de Adyacencia**: Utiliza una tabla bidimensional `n x n` para indicar con `1`s y `0`s si existe una conexión. Requiere una complejidad espacial de **Θ(n^2)**.
2. **Lista de Adyacencia**: Para cada vértice, se mantiene una lista con todos sus vértices vecinos. Su complejidad espacial es **Θ(n+m)** (donde *n* es vértices y *m* aristas).

El objetivo es elegir la mejor representación considerando el tiempo y el espacio necesario para los algoritmos a utilizar.

## Ejemplo de Representación

Tomaremos como ejemplo el grafo no dirigido mencionado en el material:

```text
  A
 / \
B   C
```

### Matriz de Adyacencia

Construimos una matriz de 3x3. Si existe una arista, colocamos `1`; en caso contrario `0`.

```text
    A B C
A [ 0 1 1 ]
B [ 1 0 0 ]
C [ 1 0 0 ]
```

### Lista de Adyacencia

Mantenemos una lista de vecinos para cada vértice:

```text
A = [B, C]
B = [A]
C = [A]
```

## Observaciones del Ejemplo

- En la matriz de adyacencia se puede verificar rápidamente si hay conexión entre dos vértices.
- En la lista de adyacencia, en un grafo no dirigido cada arista aparece dos veces (por ejemplo, A apunta a B, y B apunta a A).
- La lista de adyacencia resulta mucho más eficiente en espacio cuando el grafo es disperso (cuando tiene pocas aristas en relación con el número de vértices).

## Simulador

Para explorar y visualizar la creación de grafos y sus representaciones en memoria de forma interactiva, pueden usar este simulador:

- [VisuAlgo: Estructuras de Datos para Grafos](https://visualgo.net/en/graphds)

## Conclusiones

Los grafos son una estructura de datos ampliamente utilizada para representar relaciones complejas entre objetos. 

- Las representaciones más comunes son la **matriz de adyacencia** y la **lista de adyacencia**.
- Cada una presenta ventajas y desventajas dependiendo de la densidad del grafo y de las operaciones a realizar, por lo que analizarlos considerando tiempo y espacio es fundamental.