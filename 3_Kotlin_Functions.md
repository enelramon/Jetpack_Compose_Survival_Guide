# Funciones en Kotlin

## Índice
1. [Definiendo y Llamando](#definiendo-y-llamando)
2. [Argumentos Predeterminados y Nombrados](#argumentos-predeterminados-y-nombrados)
3. [Sobrecarga de Funciones](#sobrecarga-de-funciones)
4. [Funciones de Extensión](#funciones-de-extensión)
5. [Funciones de Orden Superior](#funciones-de-orden-superior)
6. [Expresiones Lambda](#expresiones-lambda)
7. [Funciones Inline](#funciones-inline)
8. [Funciones Recursivas](#funciones-recursivas)
9. [Funciones Recursivas de Cola](#funciones-recursivas-de-cola)
10. [Corrutinas](#corrutinas)

## Definiendo y Llamando

¡Bienvenido al fascinante mundo de las funciones en Kotlin! En este capítulo, desentrañaremos el poder y la flexibilidad que las funciones aportan a tus programas en Kotlin. Las funciones sirven como bloques de construcción, permitiéndote descomponer tu código en componentes manejables y reutilizables. Exploraremos cómo definir funciones, pasar parámetros, devolver valores y llamar funciones con diferentes enfoques. Vamos a sumergirnos en el corazón de la programación modular y eficiente en Kotlin.

## Definiendo Funciones en Kotlin

En su núcleo, una función es un bloque de código que realiza una tarea específica. En Kotlin, definir una función implica especificar su nombre, parámetros, tipo de retorno y el código que ejecuta.

### Ejemplo 1: Función Simple
```kotlin
fun saludar() {
    println("¡Hola, Desarrollador de Kotlin!")
}
```
En este ejemplo, definimos una función simple llamada `saludar`. El cuerpo de la función, encerrado en llaves, contiene el código para imprimir un mensaje de saludo.

### Ejemplo 2: Función con Parámetros
```kotlin
fun saludarPorNombre(nombre: String) {
    println("¡Hola, $nombre!")
}
```

Aquí, la función `saludarPorNombre` toma un parámetro `nombre` de tipo `String`. Esto permite que la función personalice el saludo según el nombre proporcionado.

### Ejemplo 3: Función con Tipo de Retorno
```kotlin
fun cuadrado(numero: Int): Int {
    return numero * numero
}
```

La función `cuadrado` calcula el cuadrado de un número dado y devuelve el resultado. Los dos puntos `:` seguidos de `Int` especifican el tipo de retorno de la función.

## Llamando Funciones en Kotlin

Una vez que has definido una función, llamarla o invocarla es un proceso sencillo. Proporcionas los argumentos requeridos y la función ejecuta su lógica definida.

### Ejemplo 4: Llamando Función Simple
```kotlin
// Llamando la función simple saludar
saludar()
```

Aquí, llamamos a la función `saludar`, y esta imprime el mensaje de saludo predefinido.

### Ejemplo 5: Llamando Función con Parámetros
```kotlin
// Llamando la función saludarPorNombre con un argumento
saludarPorNombre("Enel Almonte")
```

En este caso, llamamos a la función `saludarPorNombre`, pasando el argumento "Enel Almonte". La función luego imprime un saludo personalizado para Enel Almonte.

### Ejemplo 6: Llamando Función con Tipo de Retorno
```kotlin
// Llamando la función cuadrado y imprimiendo el resultado
val resultado = cuadrado(5)
println("El cuadrado de 5 es: $resultado")
```

La función `cuadrado` se llama con el argumento 5, y el resultado se almacena en la variable `resultado`. Luego imprimimos el cuadrado calculado.

## Argumentos Predeterminados y Nombrados

Kotlin te permite definir valores predeterminados para los parámetros de las funciones, haciéndolas más flexibles al llamarlas. Además, puedes usar argumentos nombrados para especificar valores para parámetros específicos.

### Ejemplo 7: Función con Argumento Predeterminado
```kotlin
fun saludarConPredeterminado(nombre: String = "Desarrollador") {
    println("¡Hola, $nombre!")
}
```
Aquí, la función `saludarConPredeterminado` tiene un valor predeterminado para el parámetro `nombre`. Si no se proporciona ningún argumento, se usa el valor predeterminado "Desarrollador".

### Ejemplo 8: Llamando Función con Argumento Predeterminado
```kotlin
// Llamando la función saludarConPredeterminado sin un argumento
saludarConPredeterminado()
```

En este ejemplo, llamar a `saludarConPredeterminado` sin proporcionar un argumento usa el valor predeterminado, resultando en el saludo "¡Hola, Desarrollador!"

### Ejemplo 9: Función con Argumentos Nombrados
```kotlin
fun mostrarOrden(articulo: String, cantidad: Int, prioridad: String = "Normal") {
    println("Artículo: $articulo, Cantidad: $cantidad, Prioridad: $prioridad")
}
```

Aquí, la función `mostrarOrden` tiene parámetros `articulo`, `cantidad` y un valor predeterminado para `prioridad`. Los argumentos nombrados nos permiten proporcionar valores en cualquier orden.

### Ejemplo 10: Llamando Función con Argumentos Nombrados
```kotlin
// Llamando la función mostrarOrden con argumentos nombrados
mostrarOrden(cantidad = 3, prioridad = "Alta", articulo = "Portátil")
```

Usando argumentos nombrados, podemos proporcionar valores para los parámetros en cualquier orden. En este ejemplo, la llamada a la función es clara y legible a pesar del orden diferente.

## Sobrecarga de Funciones

La sobrecarga de funciones en Kotlin te permite definir múltiples funciones con el mismo nombre pero diferentes listas de parámetros. El compilador determina qué función llamar en función de los argumentos proporcionados.

### Ejemplo 11: Sobrecarga de Funciones
```kotlin
fun saludar(persona: String) {
    println("¡Hola, $persona!")
}
fun saludar(persona: String, edad: Int) {
    println("¡Hola, $persona! Tienes $edad años.")
}
```

En este ejemplo, tenemos dos funciones `saludar`: una con un solo parámetro y otra con dos parámetros. La función con la lista de parámetros adecuada se llama según el contexto.

### Ejemplo 12: Llamando Funciones Sobrecargadas
```kotlin
// Llamando las funciones sobrecargadas saludar
saludar("Enel Almonte")
saludar("Bob", 30)
```

Ambas funciones `saludar` se llaman con diferentes argumentos. El compilador determina qué versión de la función invocar en función del número y tipo de argumentos.

## Funciones de Extensión

Las funciones de extensión en Kotlin te permiten agregar nuevas funciones a clases existentes sin modificar su código fuente. Esto proporciona un mecanismo poderoso para mejorar la funcionalidad de las clases.

### Ejemplo 13: Función de Extensión
```kotlin
// Extendiendo la clase String con una nueva función
fun String.agregarExclamacion(): String {
    return "$this!"
}
```

Aquí, definimos una función de extensión llamada `agregarExclamacion` para la clase `String`. Agrega un signo de exclamación a la cadena dada.

### Ejemplo 14: Usando Función de Extensión
```kotlin
// Usando la función de extensión
val saludo = "Bienvenido"
val saludoEmocionado = saludo.agregarExclamacion()
println(saludoEmocionado)
```

La función de extensión `agregarExclamacion` se usa para modificar la cadena "Bienvenido", resultando en la salida "¡Bienvenido!"

## Funciones de Orden Superior

Kotlin admite funciones de orden superior, lo que te permite pasar funciones como parámetros o devolverlas desde otras funciones. Esta característica de programación funcional mejora la legibilidad y reutilización del código.

### Ejemplo 15: Función de Orden Superior
```kotlin
// Función de orden superior que toma una función como parámetro
fun operarEnNumeros(a: Int, b: Int, operacion: (Int, Int) -> Int): Int {
    return operacion(a, b)
}

// Ejemplo de función de operación para ser pasada
fun sumar(x: Int, y: Int): Int {
    return x + y
}
```
En este ejemplo, `operarEnNumeros` es una función de orden superior que toma dos números y una función como parámetros. La función proporcionada realiza la operación deseada.

### Ejemplo 16: Usando Función de Orden Superior
```kotlin
// Usando la función de orden superior con la función sumar
val resultado = operarEnNumeros(5, 3, ::sumar)
println("Resultado de la suma: $resultado")
```

Aquí, llamamos a `operarEnNumeros` con la función `sumar` como parámetro. El resultado es la suma de 5 y 3.

## Expresiones Lambda

Las expresiones lambda en Kotlin son formas concisas de expresar funciones anónimas. Son especialmente útiles cuando se trabaja con funciones de orden superior.

### Ejemplo 17: Expresión Lambda
```kotlin
// Usando una expresión lambda en una función de orden superior
val multiplicar: (Int, Int) -> Int = { x, y -> x * y }

// Usando la función de orden superior con la expresión lambda
val producto = operarEnNumeros(4, 6, multiplicar)
println("Resultado de la multiplicación: $producto")
```
En este ejemplo, `multiplicar` es una expresión lambda que representa una función que multiplica dos números. Luego usamos esta expresión lambda en la función de orden superior `operarEnNumeros`.

## Funciones Inline

El modificador `inline` en Kotlin te permite solicitar que el compilador inserte el cuerpo de una función directamente en el código de llamada. Esto puede mejorar el rendimiento al evitar la sobrecarga de llamadas a funciones.

### Ejemplo 18: Función Inline
```kotlin
// Función inline para calcular el cuadrado de un número
inline fun cuadradoInline(numero: Int): Int {
    return numero * numero
}
```

En este ejemplo, la función `cuadradoInline` está marcada como `inline`. El compilador reemplazará la llamada a la función con su cuerpo real en el sitio de llamada.

### Ejemplo 19: Usando Función Inline
```kotlin
// Usando la función inline
val resultadoInline = cuadradoInline(8)
println("El cuadrado de 8 es: $resultadoInline")
```

La función `cuadradoInline` se usa como cualquier otra función, pero su cuerpo se inserta directamente en el sitio de llamada durante la compilación.

## Funciones Recursivas

Kotlin admite funciones recursivas, lo que permite que una función se llame a sí misma. Esto es particularmente útil para resolver problemas que pueden descomponerse en instancias más pequeñas del mismo problema.

### Ejemplo 20: Función Recursiva
```kotlin
// Función recursiva para calcular el factorial de un número
fun factorial(n: Int): Int {
    return if (n == 0 || n == 1) {
        1
    } else {
        n * factorial(n - 1)
    }
}
```
En este ejemplo, la función `factorial` calcula el factorial de un número usando recursión.

### Ejemplo 21: Usando Función Recursiva
```kotlin
// Usando la función recursiva
val resultadoFactorial = factorial(5)
println("El factorial de 5 es: $resultadoFactorial")
```

La función `factorial` se llama con el argumento 5, y el resultado se imprime.

## Funciones Recursivas de Cola

Kotlin admite la recursión de cola, una forma específica de recursión donde la llamada recursiva es la última operación realizada en la función. Las funciones recursivas de cola pueden ser optimizadas por el compilador para evitar errores de desbordamiento de pila.

### Ejemplo 22: Función Recursiva de Cola
```kotlin
// Función recursiva de cola para calcular el factorial de un número
tailrec fun factorialDeCola(n: Int, acumulador: Int = 1): Int {
    return if (n == 0) {
        acumulador
    } else {
        factorialDeCola(n - 1, n * acumulador)
    }
}
```

En este ejemplo, la función `factorialDeCola` es recursiva de cola, ya que la llamada recursiva es la última operación.

### Ejemplo 23: Usando Función Recursiva de Cola
```kotlin
// Usando la función recursiva de cola
val resultadoFactorialDeCola = factorialDeCola(5)
println("El Factorial Recursivo de Cola de 5 es: $resultadoFactorialDeCola")
```

La función `factorialDeCola` se llama con el argumento 5, y el resultado se imprime.

## Corrutinas

Kotlin introduce corrutinas, un marco de concurrencia poderoso y liviano. Las corrutinas te permiten escribir código asincrónico en un estilo secuencial, haciéndolo más fácil de razonar y mantener.

### Ejemplo 24: Función de Corrutina
```kotlin
import kotlinx.coroutines.*

// Función de corrutina para simular comportamiento asincrónico
suspend fun obtenerDatos(): String {
    delay(1000) // Simular un retraso
    return "¡Datos Obtenidos!"
}
```
En este ejemplo, la función `obtenerDatos` está marcada con `suspend`, indicando que puede ser usada en una corrutina.

### Ejemplo 25: Usando Corrutinas
```kotlin
// Usando una corrutina para llamar a la función obtenerDatos
fun main() {
    runBlocking {
        val resultado = async { obtenerDatos() }
        println("Resultado de la Corrutina: ${resultado.await()}")
    }
}
```
La función `obtenerDatos` se llama dentro de una corrutina usando `async` y `await`. La función `runBlocking` se usa para bloquear el hilo principal hasta que la corrutina complete.