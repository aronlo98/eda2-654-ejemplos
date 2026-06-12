# Arboles AVL

Este documento contiene material de apoyo para el tema de **arboles AVL** en el curso de **Estructuras de Datos 2**. Un arbol AVL es un **arbol binario de busqueda balanceado** en el que, para cada nodo, la diferencia de altura entre su subarbol izquierdo y su subarbol derecho debe mantenerse controlada.

## Propiedades

Todo arbol AVL debe cumplir lo siguiente:

1. Debe conservar la propiedad de **arbol binario de busqueda**.
2. Para cada nodo, la diferencia entre la altura del subarbol izquierdo y derecho debe ser como maximo **1**.
3. El **factor de balance** de un nodo se calcula como:

```text
factor de balance = altura(izquierdo) - altura(derecho)
```

4. Un nodo esta balanceado si su factor de balance es **-1**, **0** o **1**.
5. Si un nodo toma un valor menor que `-1` o mayor que `1`, el arbol debe **rebalancearse**.

Gracias a estas reglas, las operaciones de busqueda, insercion y eliminacion se mantienen en tiempo **O(log n)**.

## Idea General de Insercion

Cuando insertamos un nuevo elemento en un arbol AVL:

1. Se inserta como en un **BST**.
2. Se actualizan las alturas de los nodos afectados.
3. Se calcula el **factor de balance**.
4. Si aparece un desbalance, se corrige con una rotacion.

## Tipos de Rotaciones

Las rotaciones mas comunes en un arbol AVL son:

- **Rotacion simple a la derecha**: corrige un caso izquierda-izquierda.
- **Rotacion simple a la izquierda**: corrige un caso derecha-derecha.
- **Rotacion doble izquierda-derecha**: corrige un caso izquierda-derecha.
- **Rotacion doble derecha-izquierda**: corrige un caso derecha-izquierda.

Estas rotaciones reorganizan localmente el arbol sin romper el orden del BST.

## Ejemplo de Insercion

Insertaremos los elementos del arreglo:

```text
{30, 20, 10, 25, 40, 50}
```

### Paso 1: insertar 30

El arbol inicia con un solo nodo:

```text
30
```

### Paso 2: insertar 20

20 es menor que 30 y se inserta a la izquierda. El arbol sigue balanceado.

```text
  30
 /
20
```

### Paso 3: insertar 10

10 se inserta a la izquierda de 20. El nodo 30 queda con factor de balance `2`, por lo que aparece un caso **izquierda-izquierda**. Se aplica una **rotacion simple a la derecha** sobre 30.

Resultado:

```text
   20
  /  \
10   30
```

### Paso 4: insertar 25

25 es mayor que 20 y menor que 30, por lo tanto entra como hijo izquierdo de 30. El arbol sigue balanceado.

```text
   20
  /  \
10   30
     /
   25
```

### Paso 5: insertar 40

40 entra como hijo derecho de 30. No hay desbalance.

```text
   20
  /  \
10   30
     / \
   25  40
```

### Paso 6: insertar 50

50 se inserta como hijo derecho de 40. Ahora el nodo 20 queda desbalanceado hacia la derecha, formando un caso **derecha-derecha**. Se aplica una **rotacion simple a la izquierda** sobre 20.

Resultado final:

```text
      30
     /  \
   20    40
  /  \     \
10   25    50
```

## Observaciones del Ejemplo

- El arbol final mantiene la propiedad de BST.
- Todos los nodos tienen factor de balance entre **-1** y **1**.
- Se utilizaron rotaciones para evitar que el arbol creciera de forma desordenada.
- A diferencia de un BST comun, la altura se mantiene controlada.

## Simulador

Para practicar y validar resultados, pueden usar este visualizador interactivo:

- [Simulador de arbol AVL](https://www.cs.usfca.edu/~galles/visualization/AVLtree.html)

## Conclusiones

Los arboles AVL son una solucion efectiva para mantener un arbol binario de busqueda estrictamente balanceado. Su principal ventaja es que garantizan operaciones eficientes incluso cuando las inserciones podrian desordenar la estructura.

Son especialmente utiles cuando se necesita:

- **busqueda rapida**,
- **balance estricto**,
- **rendimiento predecible** en operaciones dinamicas.
