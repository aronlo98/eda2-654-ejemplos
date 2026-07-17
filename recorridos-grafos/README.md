# Recorridos en Grafos

Este documento contiene material de apoyo para el tema de **recorridos en grafos** del curso de **Estructuras de Datos 2**. 

Explorar un grafo consiste en visitar sus vértices y aristas de forma sistemática para encontrar información. Esto tiene aplicaciones clave en el mundo del desarrollo, como en los **recolectores de basura (garbage collectors)** de los lenguajes de programación, para **encontrar la mejor ruta** en un mapa o GPS, o para identificar **cuellos de botella** en una red.

## Conceptos Clave

Para recorrer un grafo sin perdernos ni quedarnos atrapados en ciclos infinitos, utilizamos dos estrategias principales:

1. **BFS (Búsqueda en Anchura / Breadth-First Search)**: Explora el grafo "nivel por nivel". Imagina que lanzas una piedra al agua y se forman ondas: primero visitas a todos los vecinos directos del nodo inicial, luego a los vecinos de esos vecinos, y así sucesivamente. (Utiliza una estructura de **Cola / Queue**).
2. **DFS (Búsqueda en Profundidad / Depth-First Search)**: Explora un camino tan profundo como sea posible hasta llegar a un callejón sin salida. Cuando ya no puede avanzar más, retrocede un paso y busca por otra rama. (Utiliza una estructura de **Pila / Stack** o recursividad).

Ambos algoritmos son muy eficientes y tienen una complejidad de **O(V + E)**, donde `V` es el número de vértices y `E` el número de aristas.

## Algoritmo Tricolor (Para Detección de Ciclos)

Una técnica avanzada y muy útil durante un recorrido DFS es clasificar los vértices con "colores" para saber exactamente en qué estado de exploración se encuentran. Esto nos permite **detectar ciclos** fácilmente:

- **Blanco**: El nodo aún no ha sido visitado.
- **Gris**: El nodo ya fue visitado, pero aún estamos explorando a sus vecinos (está "en proceso").
- **Negro**: El nodo y absolutamente todos sus vecinos han sido explorados por completo.

Si durante nuestra exploración intentamos visitar un vecino y resulta que ya es de color **Gris**, ¡hemos encontrado un ciclo! Significa que dimos una vuelta por el grafo y llegamos a un nodo del cual todavía no habíamos terminado de salir.

## ¿Cómo representamos esto en código?

### Pseudocódigo de BFS (Anchura)

```text
BFS(nodo_inicio):
    Crear una Cola Q
    Marcar nodo_inicio como visitado
    Q.encolar(nodo_inicio)

    mientras Q no este vacia:
        v = Q.desencolar()
        procesar(v)

        para cada vecino 'u' de 'v':
            si 'u' no ha sido visitado:
                marcar 'u' como visitado
                Q.encolar(u)
```

### Pseudocódigo de DFS (Profundidad usando Pila)

```text
DFS(nodo_inicio):
    Crear una Pila S
    S.apilar(nodo_inicio)

    mientras S no este vacia:
        v = S.desapilar()
        
        si 'v' no ha sido visitado:
            marcar 'v' como visitado
            procesar(v)
            
            para cada vecino 'u' de 'v':
                si 'u' no ha sido visitado:
                    S.apilar(u)
```
*(Nota: DFS también se suele implementar de forma muy elegante usando recursividad en lugar de crear una Pila manualmente).*

## Ejemplos de Ejecución (Paso a Paso)

A continuación se muestra la ejecución paso a paso de ambos algoritmos. Para estos ejemplos utilizaremos un **grafo complejo de 6 nodos** diseñado especialmente para tener convergencias (C y D apuntan a E), caminos múltiples (C apunta a E y F), y un **ciclo de regreso peligroso (F apunta a B)**, ilustrado en **arte ASCII** puro.

*Leyenda: `[ X ]` = No visitado, `* X *` = Visitado*

### Recorrido BFS (Nivel por Nivel)

**Planteamiento del Problema (BFS)**
El objetivo es recorrer este grafo partiendo del nodo `A` utilizando una estructura de Cola (Queue). Para evitar ciclos infinitos, marcamos el nodo apenas entra a la cola.

```text
           [ A ]
          /     \
         v       v
 .---> [ B ]   [ D ]
 |       |       |
 |       v       v
 |     [ C ]-->[ E ]
 |       |       |
 |       v       v
 '---- [ F ]<----'
```

**Paso 1**
Iniciamos en `A`. Lo marcamos y encolamos. (Cola: `A`)

```text
           * A *
          /     \
         v       v
 .---> [ B ]   [ D ]
 |       |       |
 |       v       v
 |     [ C ]-->[ E ]
 |       |       |
 |       v       v
 '---- [ F ]<----'
```

**Paso 2**
Desencolamos `A`. Visitamos y encolamos a sus vecinos `B` y `D`. (Cola: `B, D`)

```text
           * A *
          /     \
         v       v
 .---> * B *   * D *
 |       |       |
 |       v       v
 |     [ C ]-->[ E ]
 |       |       |
 |       v       v
 '---- [ F ]<----'
```

**Paso 3**
Desencolamos `B`. Su único vecino es `C`. Lo marcamos y encolamos. (Cola: `D, C`)

```text
           * A *
          /     \
         v       v
 .---> * B *   * D *
 |       |       |
 |       v       v
 |     * C *-->[ E ]
 |       |       |
 |       v       v
 '---- [ F ]<----'
```

**Paso 4**
Desencolamos `D`. Su vecino es `E`. Lo marcamos y encolamos. (Cola: `C, E`)

```text
           * A *
          /     \
         v       v
 .---> * B *   * D *
 |       |       |
 |       v       v
 |     * C *-->* E *
 |       |       |
 |       v       v
 '---- [ F ]<----'
```

**Paso 5**
Desencolamos `C`. Sus vecinos son `E` y `F`. Como `E` ya fue marcado en el paso anterior, solo encolamos `F`. (Cola: `E, F`)

```text
           * A *
          /     \
         v       v
 .---> * B *   * D *
 |       |       |
 |       v       v
 |     * C *-->* E *
 |       |       |
 |       v       v
 '---- * F *<----'
```

**Paso 6 y final**
Desencolamos `E` (su vecino `F` ya está marcado). Luego desencolamos `F` (su vecino `B` ¡ya estaba marcado desde el inicio!, evitando el ciclo).

**Orden BFS: A, B, D, C, E, F**

```text
           * A *
          /     \
         v       v
 .---> * B *   * D *
 |       |       |
 |       v       v
 |     * C *-->* E *
 |       |       |
 |       v       v
 '---- * F *<----'
```

### Recorrido DFS (A lo Profundo)

**Planteamiento del Problema (DFS)**
El objetivo es recorrer el mismo grafo partiendo del nodo `A` utilizando una Pila (Stack). También marcamos al entrar a la pila para evitar duplicados.

```text
           [ A ]
          /     \
         v       v
 .---> [ B ]   [ D ]
 |       |       |
 |       v       v
 |     [ C ]-->[ E ]
 |       |       |
 |       v       v
 '---- [ F ]<----'
```

**Paso 1**
Inicialmente apilamos y marcamos `A`. Lo desapilamos/visitamos, y apilamos a sus vecinos `D` y `B`. (Pila: `D, B`)

```text
           * A *
          /     \
         v       v
 .---> * B *   * D *
 |       |       |
 |       v       v
 |     [ C ]-->[ E ]
 |       |       |
 |       v       v
 '---- [ F ]<----'
```

**Paso 2**
El tope es `B`. Desapilamos `B`. Su vecino es `C`. Lo apilamos y marcamos. (Pila: `D, C`)

```text
           * A *
          /     \
         v       v
 .---> * B *   * D *
 |       |       |
 |       v       v
 |     * C *-->[ E ]
 |       |       |
 |       v       v
 '---- [ F ]<----'
```

**Paso 3**
El tope es `C`. Desapilamos `C`. Sus vecinos son `F` y `E`. Ambos los apilamos. (Pila: `D, F, E`)

```text
           * A *
          /     \
         v       v
 .---> * B *   * D *
 |       |       |
 |       v       v
 |     * C *-->* E *
 |       |       |
 |       v       v
 '---- * F *<----'
```

**Paso 4**
El tope es `E`. Desapilamos `E`. Su único vecino es `F`, pero ya está marcado (¡cruce de ramas evitado!). (Pila: `D, F`)

```text
           * A *
          /     \
         v       v
 .---> * B *   * D *
 |       |       |
 |       v       v
 |     * C *-->* E *
 |       |       |
 |       v       v
 '---- * F *<----'
```

**Paso 5**
El tope es `F`. Desapilamos `F`. Su único vecino es `B`, pero ya está marcado (¡ciclo infinito evitado!). (Pila: `D`)

```text
           * A *
          /     \
         v       v
 .---> * B *   * D *
 |       |       |
 |       v       v
 |     * C *-->* E *
 |       |       |
 |       v       v
 '---- * F *<----'
```

**Paso 6 y final**
El tope final es `D`. Desapilamos `D`. Su vecino es `E`, el cual ya está marcado, así que terminamos.

**Orden DFS: A, B, C, E, F, D**

```text
           * A *
          /     \
         v       v
 .---> * B *   * D *
 |       |       |
 |       v       v
 |     * C *-->* E *
 |       |       |
 |       v       v
 '---- * F *<----'
```

## Simuladores

Para ver animaciones interactivas de ambos algoritmos y probar con grafos más grandes, visita:
- [VisuAlgo: Recorridos DFS y BFS](https://visualgo.net/en/dfsbfs)

## Conclusiones

- **BFS** y **DFS** son las técnicas fundamentales para recorrer cualquier grafo, ambas con la misma eficiencia general `O(V + E)`.
- **BFS** es ideal para encontrar el camino más corto en grafos (sin pesos), ya que explora como una onda expansiva concéntrica.
- **DFS** es excelente para explorar caminos completos de un extremo a otro, resolver laberintos y, mediante la variante tricolor, detectar ciclos fácilmente.
