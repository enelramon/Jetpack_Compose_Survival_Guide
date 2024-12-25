# Conceptos Básicos de Kotlin: Variables, Tipos de Datos y Operadores

## Índice
1. [Variables en Kotlin](#variables-en-kotlin)
2. [Tipos de Datos en Kotlin](#tipos-de-datos-en-kotlin)
3. [Operadores en Kotlin](#operadores-en-kotlin)

Estas son las herramientas esenciales que te permiten expresar tu lógica y crear programas dinámicos y receptivos. Así que, vamos a sumergirnos y desentrañar la simplicidad y el poder de los elementos básicos de Kotlin.

## Variables en Kotlin

Las variables son como contenedores que contienen información en tu programa. Dan nombres a los valores, haciendo que tu código sea legible y adaptable. Kotlin introduce dos palabras clave principales para las variables: `val` para variables de solo lectura y `var` para variables mutables.

### Ejemplo 1: Declaración de Variables
```kotlin
val pi = 3.14
var counter = 0
```
En este ejemplo, `pi` es una variable de solo lectura inicializada con el valor 3.14, mientras que `counter` es una variable mutable que comienza en 0. La palabra clave `val` asegura que el valor de `pi` permanezca constante, mientras que `var` permite que `counter` cambie durante la ejecución del programa.

### Ejemplo 2: Inferencia de Tipos
Kotlin también es inteligente para entender los tipos de variables sin mencionarlos explícitamente.
```kotlin
val age = 25
val name = "Alice"
```
Aquí, Kotlin infiere que `age` es un entero y `name` es una cadena. Esta inferencia de tipos reduce la necesidad de declaraciones de tipos explícitas, haciendo que tu código sea conciso y legible.

## Tipos de Datos en Kotlin

Los tipos de datos definen el tipo de valores que una variable puede contener. Kotlin soporta una gama de tipos de datos, cada uno sirviendo a un propósito específico.

### Ejemplo 3: Explorando Tipos de Datos
```kotlin
val age: Int = 25
val height: Double = 175.5
val isStudent: Boolean = true
val name: String = "Alice"
```
- `age` es un entero (`Int`).
- `height` es un número de punto flotante de doble precisión (`Double`).
- `isStudent` es un booleano (`Boolean`).
- `name` es una cadena (`String`).

Entender y elegir el tipo de datos correcto es crucial para un uso eficiente de la memoria y una representación precisa de la lógica de tu programa.

## Operadores en Kotlin

Los operadores son símbolos que realizan operaciones en variables y valores. Kotlin hereda operadores familiares de Java pero los mejora para una sintaxis más expresiva y concisa.

### Ejemplo 4: Operadores Matemáticos
```kotlin
val x = 10
val y = 5
val sum = x + y
val difference = x - y
val product = x * y
val quotient = x / y
val remainder = x % y
```
En este fragmento, se realizan operaciones aritméticas básicas en las variables `x` y `y`. El resultado de cada operación se almacena en una nueva variable, mostrando la simplicidad y legibilidad de Kotlin.

### Ejemplo 5: Concatenación de Cadenas
Kotlin hace que la manipulación de cadenas sea intuitiva.
```kotlin
val firstName = "John"
val lastName = "Doe"
val fullName = firstName + " " + lastName
```
Aquí, el operador `+` concatena el primer nombre, un espacio y el apellido para crear el nombre completo.

### Ejemplo 6: Operadores de Comparación
Comparar valores es una operación común en programación.
```kotlin
val a = 10
val b = 20
val isEqual = (a == b)
val isNotEqual = (a != b)
val isGreater = (a > b)
val isLessOrEqual = (a <= b)
```
Estos operadores evalúan condiciones y devuelven un resultado booleano, ayudando en la toma de decisiones dentro de tu código.
