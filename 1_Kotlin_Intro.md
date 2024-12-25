# Conceptos Básicos de Kotlin: Variables, Tipos de Datos y Operadores

En Kotlin, declarar una variable es muy sencillo. La palabra clave `val` se usa para variables de solo lectura, y `var` para las mutables.

### Ejemplo 1: Declaración de Variables
```kotlin
val pi = 3.14
var contador = 0
```

En este ejemplo, `pi` es una variable de solo lectura inicializada con el valor 3.14, mientras que `contador` es una variable mutable que comienza en 0.

### Tipos de Datos en Kotlin
Kotlin soporta un conjunto rico de tipos de datos, incluyendo enteros, flotantes, booleanos y cadenas de texto.
El compilador infiere automáticamente el tipo, pero también puedes especificarlo explícitamente.

### Ejemplo 2: Explorando Tipos de Datos
```kotlin
val edad: Int = 25
val altura: Double = 175.5
val esEstudiante: Boolean = true
val nombre: String = "Alicia"
```

Aquí, hemos definido variables con tipos de datos específicos: `edad` como un entero, `altura` como un doble, `esEstudiante` como un booleano, y `nombre` como una cadena de texto.

### Operadores en Kotlin
Kotlin hereda operadores familiares de Java pero introduce algunas mejoras.
Vamos a explorar algunos.

### Ejemplo 3: Operadores Matemáticos
```kotlin
val x = 10
val y = 5
val suma = x + y
val diferencia = x - y
val producto = x * y
val cociente = x / y
val resto = x % y
```

### Ejemplo 4: Concatenación de Cadenas
Kotlin hace que la manipulación de cadenas sea intuitiva.
```kotlin
val primerNombre = "Juan"
val apellido = "Pérez"
val nombreCompleto = primerNombre + " " + apellido
```

Aquí, el operador `+` concatena el primer nombre, un espacio, y el apellido para crear el nombre completo.

### Ejemplo 5: Operadores de Comparación
Comparar valores es una operación común en programación.
```kotlin
val a = 10
val b = 20
val esIgual = (a == b)
val esDiferente = (a != b)
val esMayor = (a > b)
val esMenorOIgual = (a <= b)
```
Estos operadores evalúan condiciones y devuelven un resultado booleano, ayudando en la toma de decisiones dentro de tu código.

## Flujo de Control: Tomando Decisiones y Bucles
Ahora que estás familiarizado con variables, tipos de datos y operadores, exploremos el flujo de control. Esto implica tomar decisiones y recorrer el código, esencial para crear programas dinámicos y receptivos.

### Ejemplo 6: Sentencias Condicionales (if-else)
```kotlin
val temperatura = 25
if (temperatura > 30) {
    println("¡Hace calor!")
} else if (temperatura in 20..30) {
    println("El clima es agradable.")
} else {
    println("Hace un poco de frío.")
}
```

En este ejemplo, el programa decide qué imprimir basado en el valor de la variable `temperatura`. La palabra clave `in` se usa para verificar si la temperatura cae dentro de un rango específico.

### Ejemplo 7: Bucles (for y while)
```kotlin
// Bucle For
for (i en 1..5) {
    println("Cuenta: $i")
}

// Bucle While
var cuentaRegresiva = 3
while (cuentaRegresiva > 0) {
    println("Cuenta regresiva: $cuentaRegresiva")
    cuentaRegresiva--
}
```

Aquí, un bucle `for` cuenta de 1 a 5, imprimiendo el conteo en cada iteración. El bucle `while` crea una cuenta regresiva, imprimiendo el valor actual hasta que llega a cero.

## Funciones en Kotlin: Definición y Llamada

Las funciones son bloques de código reutilizables que realizan una tarea específica. Ayudan a organizar tu código y evitar la redundancia.

### Ejemplo 8: Definición y Llamada de Funciones

```kotlin
fun saludar(nombre: String) {
    println("¡Hola, $nombre!")
}

// Llamando a la función
saludar("Roberto")
```

Aquí, definimos una función `saludar` que toma un parámetro `nombre` e imprime un saludo personalizado. Llamar a la función con el argumento "Roberto" produce la salida "¡Hola, Roberto!"

### Ejemplo 9: Devolviendo Valores
```kotlin
fun cuadrado(numero: Int): Int {
    return numero * numero
}

val resultado = cuadrado(5)
println("El cuadrado de 5 es: $resultado")
```

La función `cuadrado` toma un entero `numero` como parámetro y devuelve su cuadrado.

## Programación Orientada a Objetos con Kotlin

Kotlin es un lenguaje completamente orientado a objetos, lo que te permite estructurar tu código usando clases y objetos.

### Ejemplo 10: Creando una Clase Simple

```kotlin
class Carro(val modelo: String, val año: Int) {
    fun encenderMotor() {
        println("Motor encendido para $modelo")
    }

    fun conducir() {
        println("$modelo está en movimiento!")
    }
}

// Creando una instancia de la clase Carro
val miCarro = Carro("Tesla", 2022)

// Accediendo a propiedades y llamando a métodos
println("Mi carro es un ${miCarro.modelo} del año ${miCarro.año}")
miCarro.encenderMotor()
miCarro.conducir()
```
Aquí, definimos una clase `Carro` con propiedades `modelo` y `año`, junto con métodos para encender el motor y conducir. Luego creamos una instancia de la clase (`miCarro`) e interactuamos con ella.

## Manejo de Excepciones y Gestión de Errores
En el mundo real, ocurren errores. Kotlin proporciona un sistema robusto para manejar excepciones de manera elegante.
### Ejemplo 11: Manejo de Excepciones
```kotlin
fun dividir(a: Int, b: Int): Int {
    return try {
        a / b
    } catch (e: ArithmeticException) {
        println("Error: ${e.message}")
        -1
    }
}
val resultado = dividir(10, 0)
println("Resultado de la división: $resultado")
```
La función `dividir` intenta realizar una división y captura cualquier `ArithmeticException`. En caso de error, imprime un mensaje y devuelve -1.

## Colecciones en Kotlin: Listas, Mapas y Conjuntos
Las colecciones son contenedores que contienen múltiples elementos. Kotlin proporciona un conjunto rico de tipos de colección, incluyendo listas, mapas y conjuntos.
### Ejemplo 12: Trabajando con Listas
```kotlin
val frutas = listOf("Manzana", "Banana", "Naranja")
// Accediendo a elementos
val primeraFruta = frutas[0]
// Iterando a través de la lista
for (fruta en frutas) {
    println(fruta)
}
```
Aquí, `frutas` es una lista de cadenas de texto. Accedemos al primer elemento usando notación de índice y luego iteramos a través de la lista, imprimiendo cada fruta.

### Ejemplo 13: Mapas y Conjuntos
```kotlin
val capitales = mapOf("EE.UU." a "Washington, D.C.", "Francia" a "París", "Japón" a "Tokio")
// Accediendo a valores
val capitalDeEEUU = capitales["EE.UU."]
// Trabajando con conjuntos
val numerosUnicos = setOf(1, 2, 3, 4, 5, 1, 2)
// Iterando a través del conjunto
for (numero en numerosUnicos) {
    println(numero)
}
```
En este ejemplo, `capitales` es un mapa que representa pares país-capital, y `numerosUnicos` es un conjunto sin valores duplicados.