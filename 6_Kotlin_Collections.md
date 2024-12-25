# Colecciones en Kotlin: Listas, Mapas y Conjuntos

## Índice
1. [Listas en Kotlin: Colecciones Ordenadas](#listas-en-kotlin-colecciones-ordenadas)
2. [Mapas en Kotlin: Pares Clave-Valor](#mapas-en-kotlin-pares-clave-valor)
3. [Conjuntos en Kotlin: Colecciones Únicas](#conjuntos-en-kotlin-colecciones-únicas)
4. [Funciones de Colecciones en Kotlin: Más Allá de lo Básico](#funciones-de-colecciones-en-kotlin-más-allá-de-lo-básico)
5. [Mejores Prácticas para Colecciones en Kotlin](#mejores-prácticas-para-colecciones-en-kotlin)

Las colecciones juegan un papel fundamental en la programación, facilitando el almacenamiento, recuperación y transformación de datos. En este capítulo, profundizaremos en tres tipos fundamentales de colecciones en Kotlin: Listas, Mapas y Conjuntos. Estas estructuras ofrecen capacidades únicas para adaptarse a diversos escenarios en tu viaje de codificación.

## Listas en Kotlin: Colecciones Ordenadas

Las listas son uno de los tipos de colección más comunes y versátiles en Kotlin. Una lista es una colección ordenada de elementos, que permite un fácil acceso basado en el índice de cada elemento. Vamos a explorar la creación, manipulación y utilización de listas con ejemplos prácticos.

### Ejemplo 1: Creando una Lista
```kotlin
// Creando una lista de enteros
val numeros = listOf(1, 2, 3, 4, 5)
```
En este ejemplo, se crea una lista llamada `numeros` con elementos enteros.

### Ejemplo 2: Accediendo a Elementos de la Lista
```kotlin
// Accediendo a elementos de la lista
val primerElemento = numeros[0] // 1
val ultimoElemento = numeros.last() // 5
```
Los elementos de la lista se pueden acceder usando su índice. Aquí, `primerElemento` recupera el elemento en el índice 0, y `ultimoElemento` obtiene el último elemento.

### Ejemplo 3: Modificando una Lista
```kotlin
// Modificando la lista
val listaModificada = numeros.toMutableList()
listaModificada.add(6)
listaModificada.removeAt(1)
```
Para modificar una lista, conviértela en una lista mutable (`toMutableList()`) y usa métodos como `add` y `removeAt`. En este caso, se añade un elemento y se elimina otro.

### Ejemplo 4: Iterando a Través de una Lista
```kotlin
// Iterando a través de la lista
for (numero in listaModificada) {
    println("Número Actual: $numero")
}
```
Iterar a través de una lista es simple con un bucle `for`. Este ejemplo imprime cada elemento en la lista modificada.

## Mapas en Kotlin: Pares Clave-Valor

Los mapas, también conocidos como diccionarios en algunos lenguajes, son colecciones que almacenan pares clave-valor. Cada clave en un mapa es única y está asociada con un valor específico. Esto hace que los mapas sean ideales para escenarios donde los datos necesitan ser organizados y recuperados basados en identificadores distintos.

### Ejemplo 5: Creando un Mapa
```kotlin
// Creando un mapa de países y sus capitales
val capitales = mapOf(
    "EE.UU." to "Washington, D.C.",
    "Francia" to "París",
    "Japón" to "Tokio"
)
```
En este ejemplo, se crea un mapa llamado `capitales`, asociando países con sus respectivas capitales.

### Ejemplo 6: Accediendo a Valores del Mapa
```kotlin
// Accediendo a valores en el mapa
val capitalEEUU = capitales["EE.UU."] // Washington, D.C.
val capitalDesconocida = capitales["Italia"] // null
```
Los valores en un mapa se acceden usando sus claves correspondientes. `capitalEEUU` recupera la capital de EE.UU., y `capitalDesconocida` obtiene la capital de Italia (que es `null`).

### Ejemplo 7: Modificando un Mapa
```kotlin
// Modificando el mapa
val capitalesActualizadas = capitales.toMutableMap()
capitalesActualizadas["Alemania"] = "Berlín"
capitalesActualizadas.remove("Francia")
```
Para modificar un mapa, conviértelo en un mapa mutable (`toMutableMap()`) y usa métodos como `put` y `remove`. Aquí, se añade Alemania y se elimina Francia.

### Ejemplo 8: Iterando a Través de un Mapa
```kotlin
// Iterando a través del mapa
for ((pais, capital) in capitalesActualizadas) {
    println("País: $pais, Capital: $capital")
}
```
Los mapas se pueden iterar usando un bucle `for`, con desestructuración para acceder tanto a las claves como a los valores. Este ejemplo imprime cada par país-capital.

## Conjuntos en Kotlin: Colecciones Únicas

Los conjuntos son colecciones que almacenan elementos únicos. Aseguran que cada elemento aparezca solo una vez en la colección, haciéndolos adecuados para escenarios donde la unicidad es crucial.

### Ejemplo 9: Creando un Conjunto
```kotlin
// Creando un conjunto de colores
val coloresUnicos = setOf("Rojo", "Verde", "Azul", "Rojo")
```
En este ejemplo, se crea un conjunto llamado `coloresUnicos`. Ten en cuenta que los elementos duplicados (como "Rojo") se eliminan automáticamente.

### Ejemplo 10: Comprobando la Pertenencia al Conjunto
```kotlin
// Comprobando la pertenencia al conjunto
val tieneVerde = "Verde" in coloresUnicos // true
val tieneAmarillo = "Amarillo" in coloresUnicos // false
```
La pertenencia al conjunto se puede comprobar usando el operador `in`. Aquí, `tieneVerde` comprueba si "Verde" está en el conjunto, y `tieneAmarillo` comprueba si "Amarillo" está en el conjunto.

### Ejemplo 11: Modificando un Conjunto
```kotlin
// Modificando el conjunto
val coloresModificados = coloresUnicos.toMutableSet()
coloresModificados.add("Amarillo")
coloresModificados.remove("Rojo")
```
Para modificar un conjunto, conviértelo en un conjunto mutable (`toMutableSet()`) y usa métodos como `add` y `remove`. En este caso, se añade "Amarillo" y se elimina "Rojo".

### Ejemplo 12: Iterando a Través de un Conjunto
```kotlin
// Iterando a través del conjunto
for (color in coloresModificados) {
    println("Color Actual: $color")
}
```
Los conjuntos se pueden iterar usando un bucle `for`. Este ejemplo imprime cada elemento en el conjunto modificado.

## Funciones de Colecciones en Kotlin: Más Allá de lo Básico

Kotlin proporciona un conjunto rico de funciones para trabajar con colecciones, haciendo que operaciones como filtrar, mapear y transformar datos sean convenientes y expresivas.

### Ejemplo 13: Filtrando una Lista
```kotlin
// Filtrando elementos en una lista
val numerosPares = numeros.filter { it % 2 == 0 }
```
La función `filter` se puede usar para crear una nueva lista que contenga elementos que satisfacen una condición dada. Aquí, `numerosPares` contiene solo los elementos pares de la lista `numeros`.

### Ejemplo 14: Mapeando una Lista
```kotlin
// Mapeando elementos en una lista
val numerosCuadrados = numeros.map { it * it }
```
La función `map` aplica una transformación a cada elemento en una lista, creando una nueva lista con los valores transformados. En este ejemplo, `numerosCuadrados` contiene los cuadrados de los elementos en la lista `numeros`.

### Ejemplo 15: Combinando Operaciones
```kotlin
// Combinando operaciones de filtro y mapa
val filtradosYCuadrados = numeros.filter { it % 2 == 0 }.map { it * it }
```
Operaciones como `filter` y `map` se pueden combinar para crear transformaciones más complejas. Aquí, `filtradosYCuadrados` contiene los cuadrados de los números pares de la lista `numeros`.

## Mejores Prácticas para Colecciones en Kotlin

A medida que aprovechas el poder de las colecciones en Kotlin, considera estas mejores prácticas para asegurar un código eficiente, legible y mantenible:
1. **Inmutabilidad por Defecto:** Prefiere usar colecciones inmutables (`listOf`, `setOf`, `mapOf`) a menos que se requiera explícitamente la mutabilidad. La inmutabilidad simplifica el razonamiento sobre el código y previene modificaciones no deseadas.
2. **Usa Tipos de Colección Específicos:** Elige el tipo de colección apropiado según tus requisitos. Las listas son ideales para colecciones ordenadas, los mapas para pares clave-valor y los conjuntos para elementos únicos.
3. **Aprovecha las Funciones de Colección:** Explora y usa el conjunto rico de funciones de colección proporcionadas por Kotlin (`filter`, `map`, `reduce`, etc.) para realizar operaciones complejas de manera concisa y expresiva.
4. **Considera las Implicaciones de Rendimiento:** Ten en cuenta las implicaciones de rendimiento de las operaciones de colección, especialmente en escenarios que involucran grandes conjuntos de datos. Evalúa la complejidad temporal y espacial de tu código.
5. **Seguridad Nula en Mapas:** Al tratar con valores nulos en mapas, considera usar la función `getOrDefault` para manejar escenarios donde una clave podría estar ausente.
6. **Paradigma de Programación Funcional:** Adopta el paradigma de programación funcional ofrecido por Kotlin. Funciones como `filter` y `map` se alinean con los principios de programación funcional, permitiendo un estilo de codificación más declarativo y expresivo.
7. **Encapsula Operaciones Complejas:** Para transformaciones u operaciones complejas que involucren colecciones, encapsúlalas en funciones bien nombradas. Esto promueve la legibilidad y reutilización del código.
8. **Colecciones Inmutables en Firmas de Función:** Al diseñar funciones que aceptan colecciones como parámetros, considera usar tipos de colección inmutables en la firma de la función. Esto asegura que la función no pueda modificar la colección original.