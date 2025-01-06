# Flujo de Control: Tomando Decisiones y Bucles

## Índice
1. [Declaraciones Condicionales: Tomando Decisiones Informadas](#declaraciones-condicionales-tomando-decisiones-informadas)
2. [Bucles: Repitiendo Tareas Eficientemente](#bucles-repitiendo-tareas-eficientemente)
3. [Rompiendo y Continuando Bucles](#rompiendo-y-continuando-bucles)
4. [Etiquetando Bucles](#etiquetando-bucles)
5. [Tomando Decisiones con el Flujo de Control](#tomando-decisiones-con-el-flujo-de-control)
6. [Flujo de Control Avanzado: `when` con Rangos y Patrones](#flujo-de-control-avanzado-when-con-rangos-y-patrones)
7. [Manejo de Casos Extremos: `try`, `catch` y

# Flujo de Control: Tomando Decisiones y Bucles

Exploraremos el poder de las declaraciones condicionales, como `if` y `else`, y aprenderemos a crear bucles para iterar a través del código de manera eficiente. Estos constructos son la columna vertebral de la programación dinámica y receptiva, permitiendo que tus aplicaciones en Kotlin se adapten a condiciones cambiantes e interacciones del usuario.

## Declaraciones Condicionales: Tomando Decisiones Informadas

Las declaraciones condicionales permiten que tu programa tome decisiones basadas en ciertas condiciones. La forma más común es la declaración `if`, que evalúa una condición y ejecuta un bloque de código si la condición es verdadera.

### Ejemplo 1: Declaración `if` Básica

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
<iframe src="https://pl.kotl.in/LYCtkPcai"></iframe>

En este ejemplo, el programa evalúa la temperatura e imprime un mensaje en consecuencia. El bloque `else if` permite múltiples condiciones, proporcionando un proceso de toma de decisiones más matizado.

### Ejemplo 2: Declaración `when` Expresiva

La declaración `when` es una alternativa versátil a la declaración `switch` tradicional en otros lenguajes. Evalúa múltiples condiciones y ejecuta el bloque correspondiente a la primera condición verdadera.

```kotlin
val diaDeLaSemana = "Miércoles"
when (diaDeLaSemana) {
    "Lunes" -> println("Es el comienzo de la semana.")
    "Miércoles", "Jueves" -> println("¡Mitad de semana!")
    "Sábado" -> println("¡El fin de semana está aquí!")
    else -> println("Otro día más.")
}
```

Aquí, la declaración `when` verifica el valor de `diaDeLaSemana` e imprime un mensaje basado en la condición coincidente. El bloque `else` sirve como respaldo para cualquier valor no coincidente.

## Bucles: Repitiendo Tareas Eficientemente

Los bucles son esenciales para realizar tareas repetitivas e iterar a través de colecciones o secuencias de datos. Kotlin ofrece tanto bucles `for` como `while` para adaptarse a diferentes escenarios.

### Ejemplo 3: Bucle `for`

El bucle `for` es excelente para iterar a través de rangos, matrices o cualquier objeto iterable.

```kotlin
// Imprimiendo números del 1 al 5
for (i in 1..5) {
    println("Cuenta: $i")
}
```

En este ejemplo, el bucle `for` itera a través del rango del 1 al 5, imprimiendo la cuenta en cada iteración. La palabra clave `in` simplifica la sintaxis, haciéndola más legible.

### Ejemplo 4: Bucle `while`

El bucle `while` repite un bloque de código mientras una condición especificada sea verdadera.

```kotlin
var cuentaRegresiva = 3
while (cuentaRegresiva > 0) {
    println("Cuenta regresiva: $cuentaRegresiva")
    cuentaRegresiva--
}
```

Aquí, el bucle `while` crea una cuenta regresiva imprimiendo el valor actual de `cuentaRegresiva` hasta que llega a cero. El bucle continúa mientras la condición `cuentaRegresiva > 0` sea verdadera.

### Ejemplo 5: Bucle `do-while`

Similar al bucle `while`, el bucle `do-while` ejecuta el bloque de código primero y luego verifica la condición. Esto asegura que el bloque se ejecute al menos una vez.

```kotlin
var intentos = 0
do {
    println("Intentando conectar...")
    intentos++
} while (intentos < 3)
```

En este ejemplo, el programa intenta conectarse, y el bucle continúa hasta que los `intentos` alcanzan un máximo de tres. Esto garantiza que se realice al menos un intento de conexión.

## Rompiendo y Continuando Bucles

En algunos casos, es posible que desees salir prematuramente de un bucle o saltar a la siguiente iteración. Kotlin proporciona las declaraciones `break` y `continue` para estos escenarios.

### Ejemplo 6: Declaración `break`

La declaración `break` termina el bucle más interno en el que se encuentra.

```kotlin
for (i in 1..10) {
    if (i == 5) {
        println("Rompiendo en $i")
        break
    }
    println("Iteración: $i")
}
```

En este ejemplo, el bucle itera del 1 al 10, pero cuando `i` llega a 5, se activa la declaración `break` y el bucle termina.

### Ejemplo 7: Declaración `continue`

La declaración `continue` omite el resto del código en el bucle para la iteración actual y pasa a la siguiente.

```kotlin
for (i in 1..5) {
    if (i == 3) {
        println("Saltando iteración $i")
        continue
    }
    println("Iteración: $i")
}
```

Aquí, cuando `i` es igual a 3, se ejecuta la declaración `continue`, omitiendo el resto del bucle para esa iteración.

## Etiquetando Bucles

En bucles anidados, Kotlin te permite etiquetarlos y especificar qué bucle romper o continuar.

### Ejemplo 8: `break` Etiquetado

```kotlin
bucleExterior@ for (i in 1..3) {
    for (j in 1..3) {
        if (i * j == 6) {
            println("Rompiendo en $i, $j")
            break@bucleExterior
        }
        println("Iteración: $i, $j")
    }
}
```

En este ejemplo, el bucle exterior está etiquetado como `bucleExterior`. Cuando se cumple la condición `i * j == 6`, la declaración `break@bucleExterior` termina ambos bucles.

### Ejemplo 9: `continue` Etiquetado

```kotlin
bucleExterior@ for (i in 1..3) {
    for (j in 1..3) {
        if (i == 2 && j == 2) {
            println("Saltando iteración $i, $j")
            continue@bucleExterior
        }
        println("Iteración: $i, $j")
    }
}
```

Aquí, cuando `i` es 2 y `j` es 2, la declaración `continue@bucleExterior` omite el resto del bucle interno para esa iteración.

## Tomando Decisiones con el Flujo de Control

El flujo de control se trata de tomar decisiones en tu programa, y combinar declaraciones condicionales con bucles puede crear aplicaciones poderosas y receptivas. Vamos a explorar un ejemplo más complejo que integre estos conceptos.

### Ejemplo 10: Flujo de Programa Dinámico

```kotlin
val numeros = listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
for (numero in numeros) {
    if (numero % 2 == 0) {
        println("Número par: $numero")
    } else {
        println("Número impar: $numero")
    }
}
```

En este ejemplo, el programa itera a través de una lista de números. Para cada número, verifica si es par o impar usando el operador módulo (`%`). El resultado se imprime dinámicamente, mostrando la capacidad de toma de decisiones del flujo de control.

## Flujo de Control Avanzado: `when` con Rangos y Patrones

La declaración `when` en Kotlin es una herramienta poderosa para tomar decisiones. No se limita a comprobaciones de igualdad simples; puedes usarla con rangos e incluso patrones.

### Ejemplo 11: Usando `when` con Rangos

```kotlin
val puntuacion = 85
when (puntuacion) {
    in 90..100 -> println("A")
    in 80 until 90 -> println("B")
    in 70 until 80 -> println("C")
    else -> println("Reprobado")
}
```

En este ejemplo, el programa evalúa la `puntuacion` e imprime la calificación correspondiente basada en los rangos definidos.

### Ejemplo 12: Usando `when` con Patrones

```kotlin
val resultado: Any = "Éxito"
when (resultado) {
    is String -> println("El resultado es una cadena: $resultado")
    is Int -> println("El resultado es un entero: $resultado")
    is Boolean -> println("El resultado es un booleano: $resultado")
    else -> println("Tipo de resultado desconocido")
}
```

Aquí, la declaración `when` usa patrones para verificar el tipo de la variable `resultado` e imprime un mensaje en consecuencia. La palabra clave `is` se usa para la comprobación de tipos.

## Manejo de Casos Extremos: `try`, `catch` y `finally`

En el mundo real, tus programas pueden encontrarse con situaciones inesperadas o errores. Kotlin proporciona un mecanismo robusto de manejo de excepciones con bloques `try`, `catch` y `finally`.

### Ejemplo 13: Manejo de Excepciones

```kotlin
fun dividir(a: Int, b: Int): Int {
    return try {
        a / b
    } catch (e: ArithmeticException) {
        println("Error: ${e.message}")
        -1
    } finally {
        println("Intento de división completado.")
    }
}
val resultado = dividir(10, 0)
println("Resultado de la división: $resultado")
```

En este ejemplo, la función `dividir` intenta realizar una división. Si ocurre una `ArithmeticException` (como la división por cero), el bloque `catch` maneja la excepción e imprime un mensaje de error. El bloque `finally` se ejecuta independientemente de si ocurre una excepción, proporcionando un paso de limpieza o finalización.

## Aplicación Práctica: Construyendo una Calculadora Simple

Para unir todo, vamos a construir un programa de calculadora simple usando constructos de flujo de control. Este programa aceptará la entrada del usuario para dos números y una operación, realizará el cálculo y mostrará el resultado.

### Ejemplo 14: Programa de Calculadora Simple

```kotlin
fun main() {
    println("¡Bienvenido a la Calculadora Simple!")
    print("Ingresa el primer número: ")
    val num1 = readLine()?.toDoubleOrNull() ?: run {
        println("Entrada no válida. Saliendo.")
        return
    }
    print("Ingresa el segundo número: ")
    val num2 = readLine()?.toDoubleOrNull() ?: run {
        println("Entrada no válida. Saliendo.")
        return
    }
    print("Selecciona la operación (+, -, *, /): ")
    val operacion = readLine()
    val resultado = when (operacion) {
        "+" -> num1 + num2
        "-" -> num1 - num2
        "*" -> num1 * num2
        "/" -> if (num2 != 0.0) num1 / num2 else "Error: División por cero"
        else -> "Operación no válida"
    }
    println("Resultado: $resultado")
}