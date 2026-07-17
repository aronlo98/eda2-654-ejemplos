# Ruta Mínima (Shortest Path)

Este documento contiene material de apoyo para el tema de **Ruta Mínima** en el curso de **Estructuras de Datos 2**.

## Algoritmo de Dijkstra

El algoritmo de Dijkstra encuentra el **camino de menor costo** (ruta mínima) desde un vértice origen hacia todos los demás vértices de un grafo ponderado. 
**Nota importante:** Dijkstra funciona correctamente **únicamente** cuando todos los pesos de las aristas son no negativos (≥ 0).

**Idea principal:** 
Expandir gradualmente el conjunto de vértices cuya distancia mínima desde el origen ya es conocida, utilizando una **cola de prioridad (min-heap)** para extraer siempre el vértice más cercano disponible e intentar mejorar ("relajar") las distancias de sus vecinos.

---

### Ejercicio Resuelto (Dijkstra)

Dado el siguiente grafo donde buscaremos la ruta mínima desde el origen **A**:

**Grafo inicial:**
```text
            (2)                     (3)
    A ----------------- B ----------------------- D
    |                   |                       /
    |                   |                     /
 (6)|                (1)|                   / (1)
    |                   |                 /
    |                   |               /
    C ----------------- E -------------'
            (5)
```

**Paso a paso visual:**
Partimos con todas las distancias en infinito (`∞`), excepto el origen `A` que inicia en `0`.

**Paso 1:** Empezamos en A (dist=0).
```text
            (2)                     (3)
   [A]----------------- B ----------------------- D
  (d=0)                 |                       /
    |                   |                     /
 (6)|                (1)|                   / (1)
    |                   |                 /
    |                   |               /
    C ----------------- E -------------'
            (5)
```

**Paso 2:** Extraemos A de la cola (dist=0). 
Evaluamos sus vecinos: B (`0 + 2 = 2`) y C (`0 + 6 = 6`). Se actualizan sus distancias y entran a la cola.
```text
            (2)                     (3)
   [A]================>[B]----------------------- D
  (d=0)               (d=2)                     /
    ||                  |                     /
 (6)||               (1)|                   / (1)
    \/                  |                 /
   [C]----------------- E -------------'
  (d=6)     (5)
```

**Paso 3:** De la cola (B=2, C=6), extraemos el de menor costo: **B (dist=2)**. 
Evaluamos sus vecinos: E (`2 + 1 = 3`) y D (`2 + 3 = 5`). Se actualizan sus distancias y entran a la cola.
```text
            (2)                     (3)
   [A]================>[B]======================>[D]
  (d=0)               (d=2)                     (d=5)
    ||                  ||                    /
 (6)||               (1)||                  / (1)
    \/                  \/                /
   [C]-----------------[E]-------------'
  (d=6)     (5)       (d=3)
```

**Paso 4:** De la cola (E=3, D=5, C=6), extraemos el menor: **E (dist=3)**.
Evaluamos sus vecinos: D (`3 + 1 = 4`). Como `4 < 5` (distancia actual de D), se actualiza D y entra nuevamente a la cola con prioridad 4.
El vecino C (`3 + 5 = 8`) no mejora su distancia actual de 6, por lo que se ignora.
```text
            (2)                     (3)
   [A]================>[B]----------------------> D (vía B era 5, descartado)
  (d=0)               (d=2)                       
    ||                  ||                       
 (6)||               (1)||                    // (1)
    \/                  \/                  //
   [C]-----------------[E]=================>[D]
  (d=6)     (5)       (d=3)                 (d=4)
```

**Paso 5:** Extraemos D (dist=4). No hay más nodos no visitados para mejorar.
**Paso 6:** Extraemos D (dist=5). Se ignora por estar obsoleto.
**Paso 7:** Extraemos C (dist=6). Sus vecinos no mejoran. Fin.

**Resultado final de las rutas mínimas desde A:**
- **Hacia A:** 0
- **Hacia B:** 2 *(Ruta: A -> B)*
- **Hacia E:** 3 *(Ruta: A -> B -> E)*
- **Hacia D:** 4 *(Ruta: A -> B -> E -> D)*
- **Hacia C:** 6 *(Ruta: A -> C)*

---

## Simuladores interactivos
Para probar el algoritmo paso a paso con diferentes grafos:
- [VisuAlgo - Shortest Paths (SSSP)](https://visualgo.net/es/sssp)
- [Algorithm Visualizer - Dijkstra](https://algorithm-visualizer.org/greedy/dijkstras-algorithm)

## Referencias
- Skiena, S. S. (2020). *The Algorithm Design Manual*. Springer International Publishing.
