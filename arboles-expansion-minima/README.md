# Árboles de Expansión Mínima (Minimum Weighted Spanning Tree)

Este documento contiene material de apoyo para el tema de **Árboles de Expansión Mínima (MWST)**. 

Un Árbol de Expansión Mínima es un subgrafo que:
1. Conecta todos los vértices del grafo original.
2. No contiene ciclos.
3. Su costo total (la suma de los pesos de sus aristas) es el mínimo posible.

Existen dos algoritmos principales para hallarlo: **Kruskal** y **Prim**.

---

## 1. Algoritmo de Kruskal
Kruskal ordena las aristas de menor a mayor peso y las va agregando al árbol siempre que no formen un ciclo (se apoya comúnmente en la estructura *Union-Find*).

### Ejercicio Resuelto (Kruskal)
Dado el siguiente grafo inicial de 6 vértices:

```text
        A
      /   \
   (2)     (4)
   /         \
  C ---(5)--- B
  |           |
 (3)        (10)
  |           |
  D ---(1)--- E
   \         /
  (6)      (2)
     \     /
        F
```

**Paso a paso:**
1. **Ordenar aristas por peso:** 
   `(D, E)=1`, `(A, C)=2`, `(E, F)=2`, `(C, D)=3`, `(A, B)=4`, `(B, C)=5`, `(D, F)=6`, `(B, E)=10`.

2. **Evaluar (D, E) = 1:** No forma ciclo. Se agrega.
```text
        A
      
           
   
  C           B
  
  
  
  D ===(1)=== E
  
  
     
        F
```

3. **Evaluar (A, C) = 2:** No forma ciclo. Se agrega.
```text
        A
      //  
   (2)//   
   //        
  C           B
  
  
  
  D ===(1)=== E
  
  
     
        F
```

4. **Evaluar (E, F) = 2:** No forma ciclo. Se agrega.
```text
        A
      //  
   (2)//   
   //        
  C           B
  
  
  
  D ===(1)=== E
               \\
              (2)\\
                 \\
        F
```

5. **Evaluar (C, D) = 3:** No forma ciclo. Se agrega.
```text
        A
      //  
   (2)//   
   //        
  C           B
  ||          
 (3)        
  ||          
  D ===(1)=== E
               \\
              (2)\\
                 \\
        F
```

6. **Evaluar (A, B) = 4:** No forma ciclo. Se agrega.
```text
        A
      // \\ 
   (2)//   \\(4)
   //        \\
  C           B
  ||          
 (3)        
  ||          
  D ===(1)=== E
               \\
              (2)\\
                 \\
        F
```

7. **Fin:** Ya tenemos `V - 1 = 5` aristas agregadas (donde `V` es 6). Todos los vértices están conectados.

**Costo total:** `1 + 2 + 2 + 3 + 4 = 12`.

---

## 2. Algoritmo de Prim
Prim comienza en un vértice cualquiera y en cada paso expande el árbol agregando la arista más barata que conecte un vértice ya visitado con uno no visitado.

### Ejercicio Resuelto (Prim)
Usando el mismo grafo inicial:

```text
        A
      /   \
   (2)     (4)
   /         \
  C ---(5)--- B
  |           |
 (3)        (10)
  |           |
  D ---(1)--- E
   \         /
  (6)      (2)
     \     /
        F
```

**Paso a paso:**

1. **Elegir un vértice inicial (ej. A).**
   * *Visitados = {A}*.
```text
       [A]
      
           
   
  C           B
  
  
  
  D           E
  
  
     
        F
```

2. **Aristas disponibles desde {A}:** `(A, C)=2`, `(A, B)=4`.
   * Elegimos la de menor costo: **(A, C) = 2**. 
   * *Visitados = {A, C}*.
```text
       [A]
      //  
   (2)//   
   //        
 [C]          B
  
  
  
  D           E
  
  
     
        F
```

3. **Aristas disponibles desde {A, C}:** `(A, B)=4`, `(B, C)=5`, `(C, D)=3`.
   * Elegimos la de menor costo: **(C, D) = 3**. 
   * *Visitados = {A, C, D}*.
```text
       [A]
      //  
   (2)//   
   //        
 [C]          B
  ||          
 (3)        
  ||          
 [D]          E
  
  
     
        F
```

4. **Aristas disponibles desde {A, C, D}:** `(A, B)=4`, `(B, C)=5`, `(D, E)=1`, `(D, F)=6`.
   * Elegimos la de menor costo: **(D, E) = 1**. 
   * *Visitados = {A, C, D, E}*.
```text
       [A]
      //  
   (2)//   
   //        
 [C]          B
  ||          
 (3)        
  ||          
 [D]===(1)===[E]
  
  
     
        F
```

5. **Aristas disponibles desde {A, C, D, E}:** `(A, B)=4`, `(B, C)=5`, `(B, E)=10`, `(D, F)=6`, `(E, F)=2`.
   * Elegimos la de menor costo: **(E, F) = 2**. 
   * *Visitados = {A, C, D, E, F}*.
```text
       [A]
      //  
   (2)//   
   //        
 [C]          B
  ||          
 (3)        
  ||          
 [D]===(1)===[E]
               \\
              (2)\\
                 \\
       [F]
```

6. **Aristas disponibles desde {A, C, D, E, F} hacia nodos no visitados (solo queda B):** `(A, B)=4`, `(B, C)=5`, `(B, E)=10`.
   * Elegimos la de menor costo: **(A, B) = 4**.
   * *Visitados = {A, B, C, D, E, F}*.
```text
       [A]
      // \\ 
   (2)//   \\(4)
   //        \\
 [C]         [B]
  ||          
 (3)        
  ||          
 [D]===(1)===[E]
               \\
              (2)\\
                 \\
       [F]
```

7. **Fin:** Todos los vértices han sido visitados.

**Costo total:** `2 + 3 + 1 + 2 + 4 = 12`.

---

## Simuladores interactivos
Para probar con otros grafos paso a paso:
- [VisuAlgo - Árbol de Expansión Mínima (Kruskal y Prim)](https://visualgo.net/es/mst)
- [Algorithm Visualizer - Kruskal](https://algorithm-visualizer.org/greedy/kruskals-minimum-spanning-tree)
- [Algorithm Visualizer - Prim](https://algorithm-visualizer.org/greedy/prims-minimum-spanning-tree)
