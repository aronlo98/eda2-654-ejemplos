# Arboles Rojos-Negros

Este documento contiene material de apoyo para el tema de **arboles rojos-negros** en el curso de **Estructuras de Datos 2**. Un arbol rojo-negro es un **arbol binario de busqueda balanceado** que mantiene sus operaciones de busqueda, insercion y eliminacion en tiempo **O(log n)**.

A diferencia de un arbol AVL, el balance en un arbol rojo-negro no se controla con un factor de balance, sino con **colores** y **rotaciones**. Esto permite un balance menos estricto, pero con menos rotaciones en muchas inserciones y eliminaciones.

## Propiedades

Todo arbol rojo-negro debe cumplir estas reglas:

1. Cada nodo es **rojo** o **negro**.
2. La **raiz siempre es negra**.
3. Todas las hojas vacias o nodos **NIL** son negras.
4. Un nodo rojo **no puede tener un hijo rojo**.
5. Todo camino desde un nodo hasta un nodo **NIL** contiene la misma cantidad de nodos negros.

Estas propiedades garantizan que la altura del arbol se mantenga acotada y, por tanto, las operaciones sigan siendo eficientes.

## Idea General de Insercion

Cuando insertamos un nuevo elemento:

1. Se inserta como en un **arbol binario de busqueda**.
2. El nuevo nodo se colorea inicialmente de **rojo**.
3. Si se rompe alguna propiedad, se corrige usando:
   - **recoloreo**, o
   - **rotacion izquierda / derecha**.

El objetivo es restaurar las propiedades del arbol rojo-negro sin perder el orden del BST.

## Ejemplo de Insercion

Insertaremos los elementos del arreglo:

```text
{8, 18, 5, 15, 17, 25, 40, 30}
```

### Paso 1: insertar 8

El primer nodo se convierte en la raiz, por lo tanto queda negro.

```text
8(B)
```

### Paso 2: insertar 18

18 es mayor que 8, se inserta a la derecha como rojo.

```text
8(B)
  \
  18(R)
```

### Paso 3: insertar 5

5 es menor que 8, se inserta a la izquierda como rojo. No hay conflicto.

```text
    8(B)
   /   \
 5(R) 18(R)
```

### Paso 4: insertar 15

15 va como hijo izquierdo de 18. Aparece conflicto rojo-rojo entre 18 y 15, pero el tio (5) tambien es rojo. Entonces:

- se recolorean 5 y 18 a negro,
- 8 pasa temporalmente a rojo,
- la raiz vuelve a negro.

```text
    8(B)
   /   \
 5(B) 18(B)
      /
   15(R)
```

### Paso 5: insertar 17

17 entra como hijo derecho de 15. El tio es negro, asi que hay que rotar:

- rotacion izquierda en 15,
- luego rotacion derecha en 18,
- recoloreo.

Resultado:

```text
    8(B)
   /   \
 5(B) 17(B)
      /   \
   15(R) 18(R)
```

### Paso 6: insertar 25

25 entra como hijo derecho de 18. El padre es rojo y el tio (15) tambien es rojo, asi que se recolorea:

- 15 y 18 pasan a negro,
- 17 pasa a rojo.

```text
    8(B)
   /   \
 5(B) 17(R)
      /   \
   15(B) 18(B)
             \
             25(R)
```

### Paso 7: insertar 40

40 entra a la derecha de 25. El padre es rojo y el tio es negro, por lo que se aplica una rotacion y recoloreo.

```text
    8(B)
   /   \
 5(B) 17(R)
      /   \
   15(B) 25(B)
          /   \
       18(R) 40(R)
```

### Paso 8: insertar 30

30 entra como hijo izquierdo de 40. Esto provoca nuevas correcciones que terminan recoloreando y reorganizando el arbol. El resultado final es:

```text
        17(B)
       /     \
    8(R)     25(R)
   /   \     /    \
 5(B) 15(B) 18(B) 40(B)
                    /
                 30(R)
```

## Observaciones del Ejemplo

- La raiz final es **17** y es negra.
- No hay dos nodos rojos consecutivos.
- Todos los caminos desde la raiz hasta los nodos NIL tienen la misma cantidad de nodos negros.
- El arbol se mantiene balanceado sin exigir un balance tan estricto como AVL.

## Simulador

Para practicar y verificar resultados, pueden usar este simulador interactivo:

- [Simulador de arbol rojo-negro](https://ds2-iiith.vlabs.ac.in/exp/red-black-tree/red-black-tree-oprations/simulation/redblack.html)

## Conclusiones

Los arboles rojos-negros son una excelente alternativa para mantener colecciones ordenadas con buen rendimiento. Su fortaleza principal es combinar:

- **busqueda binaria**,
- **balanceo eficiente**,
- **menos rotaciones que AVL** en muchos casos.

Por eso son una estructura muy utilizada en implementaciones reales de mapas, conjuntos y tablas ordenadas.
