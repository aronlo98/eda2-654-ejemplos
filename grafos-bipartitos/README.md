# Grafos Bipartitos

Este documento contiene material de apoyo para el tema de **grafos bipartitos** del curso de **Estructuras de Datos 2**. 

Piensa en un grafo bipartito como dos grupos completamente distintos de cosas (por ejemplo, "estudiantes" y "cursos", o "médicos" y "hospitales"). La regla de oro es que **solo puede haber conexiones entre elementos de grupos distintos**, nunca entre miembros del mismo grupo. Esto los hace ideales para modelar aplicaciones del mundo real, como sistemas de recomendación o plataformas de citas.

## Conceptos Clave

1. **Emparejamiento (Matching)**: Es simplemente el acto de formar parejas, uniendo un elemento del primer grupo con uno del segundo.
2. **Las Preferencias**: En la vida real, las conexiones no son aleatorias. Cada persona tiene una "lista de deseos" ordenada de quién le gusta más del otro grupo.
3. **Pareja de Bloqueo**: Es una situación conflictiva. Ocurre cuando dos personas no están juntas, pero en secreto **ambas se prefieren mutuamente** antes que a sus parejas asignadas. 
4. **Matrimonio Estable**: Es un emparejamiento perfecto donde **no existe ninguna pareja de bloqueo**. Es decir, nadie tiene motivos para abandonar a su pareja actual.

## ¿Cómo representamos esto en código?

Para que la computadora entienda quién prefiere a quién, usamos matrices `n x n`. Hay dos enfoques principales:

1. **Matriz de Preferencias (`gp`)**: Es la lista de deseos tal cual. En la fila de una chica, la columna 1 tiene a su chico favorito, la 2 al segundo, etc. Es fácil de entender para nosotros, pero para la computadora es lento (toma tiempo `O(n)`) averiguar si un chico es mejor que otro.
2. **Matriz de Ranking (`gr`)**: En lugar del orden, guarda "calificaciones". En la intersección de una chica y un chico, se guarda qué puesto ocupa ese chico en su lista. Esto permite comparar parejas al instante (en tiempo `O(1)`).

Por lo general, en los algoritmos usamos la Matriz de Ranking porque agiliza muchísimo las comparaciones.

## El Algoritmo de Gale-Shapley

Gale-Shapley es un algoritmo famoso (con complejidad `O(n^2)`) que **garantiza** encontrar un emparejamiento estable. Imagina que el grupo A (chicas) debe proponerle al grupo B (chicos):

1. Mientras haya una chica soltera, ella va y se le declara al primer chico de su lista que aún no ha intentado rechazarla.
2. Si el chico está **soltero**, él acepta temporalmente.
3. Si el chico **ya tiene pareja**, él evalúa: 
   - *¿Me gusta más mi pareja actual o esta nueva chica que se me declara?*
   - Si prefiere a la **nueva**, rompe con su novia (quien vuelve a estar soltera) y se queda con la nueva.
   - Si prefiere a su **pareja actual**, rechaza a la nueva chica, y ella tendrá que ir a intentar con el siguiente en su lista.

Este ciclo se repite hasta que todos tienen pareja. Matemáticamente, este proceso asegura que al final del día **no habrá parejas inestables**.

## Ejemplos de Ejecución (Paso a Paso)

A continuación resolveremos detalladamente dos ejercicios de emparejamientos utilizando el algoritmo de Gale-Shapley (donde las chicas siempre son las que proponen).

### Ejemplo 1: Efecto dominó

Supongamos que tenemos 4 chicas y 4 chicos, con sus respectivas matrices de preferencias (donde la 1ra opción es la favorita):

**Preferencias de las Chicas**
| Chica | 1ra | 2da | 3ra | 4ta |
|---|---|---|---|---|
| **1** | 1 | 2 | 3 | 4 |
| **2** | 2 | 1 | 4 | 3 |
| **3** | 1 | 3 | 2 | 4 |
| **4** | 3 | 4 | 1 | 2 |

**Preferencias de los Chicos**
| Chico | 1ra | 2da | 3ra | 4ta |
|---|---|---|---|---|
| **1** | 2 | 3 | 1 | 4 |
| **2** | 1 | 2 | 4 | 3 |
| **3** | 4 | 3 | 2 | 1 |
| **4** | 1 | 4 | 2 | 3 |

#### Resolución paso a paso:

1. La **Chica 1** se le declara a su favorito: **Chico 1**. Como él está libre, acepta. *Parejas: (Ch1, C1)*
2. La **Chica 2** se le declara a su favorito: **Chico 2**. Como él está libre, acepta. *Parejas: (Ch1, C1), (Ch2, C2)*
3. La **Chica 3** se le declara a su favorito: **Chico 1**. El Chico 1 ya está con la Chica 1, pero revisa su lista: `[2, 3, 1, 4]`. Prefiere a la Chica 3 (su 2da opción) sobre la Chica 1 (su 3ra opción). Rompe con la Chica 1 y acepta a la Chica 3. *Parejas: (Ch3, C1), (Ch2, C2).* (La Chica 1 vuelve a estar libre).
4. La **Chica 1** (libre) intenta con su segunda opción: **Chico 2**. El Chico 2 está con la Chica 2. Su lista es `[1, 2, 4, 3]`. Prefiere a la Chica 1 sobre la Chica 2. Rompe con la Chica 2 y acepta a la Chica 1. *Parejas: (Ch3, C1), (Ch1, C2).* (La Chica 2 vuelve a estar libre).
5. La **Chica 2** (libre) intenta con su segunda opción: **Chico 1**. El Chico 1 está con la Chica 3. Su lista es `[2, 3, 1, 4]`. ¡La Chica 2 es su favorita absoluta! Rompe con la Chica 3 y acepta a la Chica 2. *Parejas: (Ch2, C1), (Ch1, C2).* (La Chica 3 vuelve a estar libre).
6. La **Chica 3** (libre) intenta con su segunda opción: **Chico 3**. Está libre, acepta de inmediato. *Parejas: (Ch2, C1), (Ch1, C2), (Ch3, C3)*
7. La **Chica 4** se le declara a su favorito: **Chico 3**. El Chico 3 está con la Chica 3. Su lista es `[4, 3, 2, 1]`. Prefiere a la Chica 4 (su 1ra opción) sobre la Chica 3. Rompe con la Chica 3 y acepta a la Chica 4. *Parejas: (Ch2, C1), (Ch1, C2), (Ch4, C3).* (La Chica 3 vuelve a estar libre).
8. La **Chica 3** (libre) intenta con su tercera opción: **Chico 2**. El Chico 2 está con la Chica 1. Su lista es `[1, 2, 4, 3]`. Prefiere a su actual pareja (Chica 1) frente a la Chica 3. El Chico 2 rechaza a la Chica 3.
9. La **Chica 3** intenta con su cuarta opción: **Chico 4**. El Chico 4 está libre, acepta de inmediato.

**Emparejamiento final:** `(Chica 2, Chico 1), (Chica 1, Chico 2), (Chica 4, Chico 3), (Chica 3, Chico 4)`

---

### Ejemplo 2: Rechazos rápidos

Para un nuevo caso, observemos las siguientes matrices de preferencias:

**Preferencias de las Chicas**
| Chica | 1ra | 2da | 3ra | 4ta |
|---|---|---|---|---|
| **1** | 2 | 1 | 4 | 3 |
| **2** | 2 | 3 | 1 | 4 |
| **3** | 1 | 2 | 3 | 4 |
| **4** | 1 | 4 | 3 | 2 |

**Preferencias de los Chicos**
| Chico | 1ra | 2da | 3ra | 4ta |
|---|---|---|---|---|
| **1** | 2 | 4 | 3 | 1 |
| **2** | 3 | 1 | 2 | 4 |
| **3** | 1 | 2 | 4 | 3 |
| **4** | 4 | 2 | 3 | 1 |

#### Resolución paso a paso:

1. La **Chica 1** propone a su favorito: **Chico 2**. Está libre, acepta. *Parejas: (Ch1, C2)*
2. La **Chica 2** propone a su favorito: **Chico 2**. El Chico 2 ya está con la Chica 1. Revisa su lista: `[3, 1, 2, 4]`. Prefiere a la Chica 1 (su 2da opción) sobre la Chica 2 (su 3ra opción). ¡El Chico 2 rechaza de inmediato a la Chica 2!
3. La **Chica 2** (sigue libre) propone a su segunda opción: **Chico 3**. Está libre, acepta. *Parejas: (Ch1, C2), (Ch2, C3)*
4. La **Chica 3** propone a su favorito: **Chico 1**. Está libre, acepta. *Parejas: (Ch1, C2), (Ch2, C3), (Ch3, C1)*
5. La **Chica 4** propone a su favorito: **Chico 1**. El Chico 1 está con la Chica 3. Su lista es `[2, 4, 3, 1]`. Prefiere a la Chica 4 (su 2da opción) sobre la Chica 3 (su 3ra opción). Rompe con la Chica 3 y acepta a la Chica 4. *Parejas: (Ch1, C2), (Ch2, C3), (Ch4, C1).* (La Chica 3 vuelve a quedar libre).
6. La **Chica 3** (libre) propone a su segunda opción: **Chico 2**. El Chico 2 está con la Chica 1. Su lista es `[3, 1, 2, 4]`. ¡La Chica 3 es su favorita absoluta (1ra opción)! Rompe con la Chica 1 y acepta a la Chica 3. *Parejas: (Ch3, C2), (Ch2, C3), (Ch4, C1).* (La Chica 1 vuelve a quedar libre).
7. La **Chica 1** (libre) propone a su segunda opción: **Chico 1**. El Chico 1 está con la Chica 4. Su lista es `[2, 4, 3, 1]`. Prefiere a su novia actual (Chica 4) sobre la Chica 1. El Chico 1 rechaza a la Chica 1.
8. La **Chica 1** propone a su tercera opción: **Chico 4**. Está libre, acepta de inmediato. 

Ya no quedan chicas libres y hemos logrado el **emparejamiento final:**
`(Chica 3, Chico 2), (Chica 2, Chico 3), (Chica 4, Chico 1), (Chica 1, Chico 4)`

## Simulador

Para explorar y entender la ejecución de este algoritmo de forma interactiva con animaciones visuales, puedes utilizar el siguiente recurso:

- [Visualizador del Algoritmo Gale-Shapley (Hudson G)](https://www.hudsong.dev/gale-shapley)

## Conclusiones

- Los **grafos bipartitos** son la base para resolver problemas de asignación (quién se queda con qué).
- La **estabilidad** es lo que hace que una asignación perdure sin conflictos.
- **Gale-Shapley** es un algoritmo elegante que soluciona este problema de manera garantizada y rápida.