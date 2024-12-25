# Programación Orientada a Objetos
La Programación Orientada a Objetos gira en torno al concepto de objetos, que son instancias de clases, y Kotlin integra estos conceptos perfectamente en su sintaxis. Exploraremos clases, objetos, herencia, polimorfismo, encapsulamiento y más. Vamos a sumergirnos en los fundamentos de la POO y cómo se manifiestan en Kotlin.

## Clases y Objetos: Los Bloques de Construcción de la POO
En el núcleo de la Programación Orientada a Objetos están las clases y los objetos. Una clase es un plano o plantilla que define la estructura y el comportamiento de los objetos. Los objetos, por otro lado, son instancias de clases, que representan entidades del mundo real. Kotlin simplifica la creación de clases y objetos, lo que lo convierte en un lenguaje ideal para la POO.

### Ejemplo 1: Creando una Clase Simple
```kotlin
// Definición de la clase Carro
class Carro {
    var marca: String = ""
    var modelo: String = ""
    var anio: Int = 0

    fun mostrarInfo() {
        println("Carro: $marca $modelo, Año: $anio")
    }
}
```

En este ejemplo, definimos una clase `Carro` con propiedades (`marca`, `modelo`, `anio`) y un método (`mostrarInfo`) para mostrar información sobre el carro.

### Ejemplo 2: Creando Objetos a partir de una Clase
```kotlin
// Creando objetos de la clase Carro
val carro1 = Carro()
carro1.marca = "Toyota"
carro1.modelo = "Camry"
carro1.anio = 2022

val carro2 = Carro()
carro2.marca = "Honda"
carro2.modelo = "Civic"
carro2.anio = 2021

// Llamando al método mostrarInfo en cada objeto carro
carro1.mostrarInfo()
carro2.mostrarInfo()
```

Aquí, creamos dos objetos (`carro1` y `carro2`) de la clase `Carro` y establecemos sus propiedades. Luego se llama al método `mostrarInfo` en cada objeto para mostrar su información.

## Constructores: Inicializando Objetos con Facilidad
Los constructores son métodos especiales en una clase que son responsables de inicializar las propiedades de un objeto cuando se crea. Kotlin ofrece formas concisas y flexibles para definir constructores.

### Ejemplo 3: Constructor Primario
```kotlin
// Clase con un constructor primario
class Libro(val titulo: String, val autor: String, val anio: Int) {
    // Método para mostrar información sobre el libro
    fun mostrarInfo() {
        println("Libro: $titulo por $autor, Año: $anio")
    }
}
```

En este ejemplo, definimos una clase `Libro` con un constructor primario. Las propiedades (`titulo`, `autor`, `anio`) se declaran directamente en el encabezado del constructor.

### Ejemplo 4: Creando Objetos con Constructor Primario
```kotlin
// Creando objetos usando el constructor primario
val libro1 = Libro("El Alquimista", "Paulo Coelho", 1988)
val libro2 = Libro("Matar a un ruiseñor", "Harper Lee", 1960)

// Llamando al método mostrarInfo en cada objeto libro
libro1.mostrarInfo()
libro2.mostrarInfo()
```

Se crean objetos (`libro1` y `libro2`) usando el constructor primario, proporcionando valores para las propiedades. Luego se llama al método `mostrarInfo` para mostrar información sobre cada libro.

### Ejemplo 5: Constructor Secundario
```kotlin
class Pelicula {
    var titulo: String = ""
    var director: String = ""
    var anio: Int = 0

    // Constructor secundario con parámetros adicionales
    constructor(titulo: String, director: String, anio: Int) {
        this.titulo = titulo
        this.director = director
        this.anio = anio
    }

    // Método para mostrar información sobre la película
    fun mostrarInfo() {
        println("Pelicula: $titulo dirigida por $director, Año: $anio")
    }
}
```

Aquí, definimos una clase `Pelicula` con un constructor secundario que permite pasar parámetros adicionales al crear un objeto.

### Ejemplo 6: Creando Objetos con Constructor Secundario
```kotlin
// Creando objetos usando el constructor secundario
val pelicula1 = Pelicula("Inception", "Christopher Nolan", 2010)
val pelicula2 = Pelicula("Sueño de fuga", "Frank Darabont", 1994)

// Llamando al método mostrarInfo en cada objeto película
pelicula1.mostrarInfo()
pelicula2.mostrarInfo()
```

Se crean objetos (`pelicula1` y `pelicula2`) usando el constructor secundario, proporcionando valores para los parámetros adicionales. El método `mostrarInfo` muestra información sobre cada película.

## Herencia: Construyendo Jerarquías de Clases
La herencia es un concepto fundamental en la POO que permite que una clase herede propiedades y comportamientos de otra clase. Kotlin soporta la herencia de una sola clase, proporcionando una forma limpia y estructurada de construir jerarquías de clases.

### Ejemplo 7: Heredando de una Clase Base
```kotlin
// Clase base
open class Animal(val nombre: String) {
    open fun hacerSonido() {
        println("Animal $nombre hace un sonido genérico")
    }
}

// Clase derivada que hereda de Animal
class Perro(nombre: String) : Animal(nombre) {
    // Sobrescribiendo el método hacerSonido
    override fun hacerSonido() {
        println("Perro $nombre ladra fuerte")
    }
}
```

En este ejemplo, definimos una clase base (`Animal`) con una propiedad (`nombre`) y un método (`hacerSonido`). La clase `Perro` hereda de `Animal` y sobrescribe el método `hacerSonido`.

### Ejemplo 8: Usando Clases Heredadas
```kotlin
// Creando objetos de las clases base y derivada
val animalGenerico = Animal("Genérico")
val perroPeludo = Perro("Peludo")

// Llamando al método hacerSonido en cada objeto
animalGenerico.hacerSonido()
perroPeludo.hacerSonido()
```

Se crean objetos (`animalGenerico` y `perroPeludo`) de las clases base y derivada. Se llama al método `hacerSonido` en cada objeto, demostrando polimorfismo.

## Polimorfismo: Abrazando la Diversidad en Tipos
El polimorfismo permite que objetos de diferentes tipos sean tratados como objetos de un tipo base común. Kotlin soporta el polimorfismo a través de la herencia de clases y la implementación de interfaces.

### Ejemplo 9: Usando Polimorfismo con Interfaces
```kotlin
// Interfaz que define el comportamiento de sonido
interface HacedorDeSonido {
    fun hacerSonido()
}

// Clase que implementa la interfaz HacedorDeSonido
class Gato(val nombre: String) : HacedorDeSonido {
    // Implementando el método hacerSonido
    override fun hacerSonido() {
        println("Gato $nombre ronronea suavemente")
    }
}
```

Aquí, definimos una interfaz `HacedorDeSonido` con un método `hacerSonido`. La clase `Gato` implementa esta interfaz.

### Ejemplo 10: Usando Polimorfismo con Objetos de Interfaz
```kotlin
// Creando objetos de la interfaz y la clase que la implementa
val hacedorDeSonido: HacedorDeSonido = Gato("Bigotes")

// Llamando al método hacerSonido en el objeto de la interfaz
hacedorDeSonido.hacerSonido()
```

Se crea un objeto (`hacedorDeSonido`) de la interfaz `HacedorDeSonido`, apuntando a una instancia de `Gato`. Se llama al método `hacerSonido` en el objeto de la interfaz, mostrando polimorfismo.

## Encapsulamiento: Protegiendo el Mundo Interior
El encapsulamiento es la práctica de agrupar los datos (propiedades) y los métodos que operan sobre los datos dentro de una sola unidad, conocida como clase. Kotlin proporciona modificadores de visibilidad para controlar la accesibilidad de las propiedades y métodos.

### Ejemplo 11: Usando Modificadores de Visibilidad
```kotlin
// Clase con propiedades privadas y públicas
class CuentaBancaria {
    private var saldo: Double = 0.0

    // Método público para depositar dinero
    fun depositar(monto: Double) {
        if (monto > 0) {
            saldo += monto
            println("Depósito exitoso. Nuevo saldo: $saldo")
        } else {
            println("Monto de depósito inválido")
        }
    }

    // Método público para verificar el saldo
    fun verificarSaldo() {
        println("Saldo actual: $saldo")
    }
}
```

En este ejemplo, la clase `CuentaBancaria` tiene una propiedad privada (`saldo`) y métodos públicos (`depositar`, `verificarSaldo`). El `saldo` está encapsulado y su acceso está restringido a los métodos dentro de la clase.

### Ejemplo 12: Interactuando con la Clase Encapsulada
```kotlin
// Creando un objeto de la clase CuentaBancaria
val cuenta = CuentaBancaria()

// Realizando operaciones en el objeto
cuenta.depositar(1000.0)
cuenta.depositar(-500.0) // Monto de depósito inválido
cuenta.verificarSaldo()
```

Se crea un objeto (`cuenta`) de la clase `CuentaBancaria`, y se realizan operaciones como depositar dinero y verificar el saldo. El encapsulamiento asegura que la propiedad `saldo` sea accedida y modificada solo a través de los métodos de la clase.

## Clases Abstractas e Interfaces: Plano para Tipos
Las clases abstractas y las interfaces proporcionan planos para que otras clases los sigan. Las clases abstractas pueden tener métodos abstractos y concretos, mientras que las interfaces solo declaran métodos abstractos.

### Ejemplo 13: Clase Abstracta
```kotlin
// Clase abstracta con métodos abstractos y concretos
abstract class Forma(val nombre: String) {
    // Método abstracto para calcular el área
    abstract fun calcularArea(): Double

    // Método concreto
    fun mostrarInfo() {
        println("$nombre - Área: ${calcularArea()}")
    }
}
```

Aquí, definimos una clase abstracta (`Forma`) con un método abstracto (`calcularArea`) y un método concreto (`mostrarInfo`).

### Ejemplo 14: Clase Concreta que Extiende la Clase Abstracta
```kotlin
// Clase concreta que extiende la clase abstracta
class Circulo(val radio: Double) : Forma("Círculo") {
    // Sobrescribiendo el método abstracto para calcular el área
    override fun calcularArea(): Double {
        return 3.14 * radio * radio
    }
}
```

La clase `Circulo` extiende la clase abstracta `Forma` y proporciona una implementación para el método abstracto.

### Ejemplo 15: Usando Clase Abstracta y Clase Concreta
```kotlin
// Creando un objeto de la clase concreta
val circulo = Circulo(5.0)

// Llamando a los métodos concretos y abstractos
circulo.mostrarInfo()
```

Se crea un objeto (`circulo`) de la clase `Circulo`, y se llama al método `mostrarInfo`, mostrando el uso de métodos abstractos y concretos.

### Ejemplo 16: Interfaz
```kotlin
// Interfaz que define el comportamiento de una impresora
interface Impresora {
    fun imprimir(documento: String)
}
```

Aquí, definimos una interfaz `Impresora` con un solo método abstracto (`imprimir`).

### Ejemplo 17: Clase que Implementa una Interfaz
```kotlin
// Clase que implementa la interfaz Impresora
class ImpresoraLaser : Impresora {
    // Implementando el método imprimir
    override fun imprimir(documento: String) {
        println("Imprimiendo documento: $documento (Impresora Láser)")
    }
}
```

La clase `ImpresoraLaser` implementa la interfaz `Impresora` y proporciona una implementación para el método `imprimir`.

### Ejemplo 18: Usando Interfaz y Clase que la Implementa
```kotlin
// Creando un objeto de la clase que implementa la interfaz
val impresoraLaser = ImpresoraLaser()

// Llamando al método de la interfaz a través de la clase que la implementa
impresoraLaser.imprimir("Informe de Ventas")
```

Se crea un objeto (`impresoraLaser`) de la clase `ImpresoraLaser`, y se llama al método `imprimir`, demostrando el uso de interfaces.

## Data Classes: Simplificando la Creación de Clases de Datos
Las data classes en Kotlin son clases diseñadas para almacenar datos. Proporcionan una forma concisa de crear clases con propiedades, y automáticamente generan métodos como `equals`, `hashCode`, y `toString`.

### Ejemplo 19: Definiendo una Data Class
```kotlin
// Definición de una data class
data class Persona(val nombre: String, val edad: Int)
```

En este ejemplo, definimos una data class `Persona` con propiedades `nombre` y `edad`.

### Ejemplo 20: Usando una Data Class
```kotlin
// Creando objetos de la data class
val persona1 = Persona("Juan", 30)
val persona2 = Persona("Ana", 25)

// Usando los métodos generados automáticamente
println(persona1)
println(persona2)
println(persona1 == persona2)
```

Se crean objetos (`persona1` y `persona2`) de la data class `Persona`, y se utilizan los métodos generados automáticamente como `toString` y `equals`.

## Sealed Interfaces: Definiendo Jerarquías de Tipos Restringidas
Las sealed interfaces en Kotlin permiten definir jerarquías de tipos restringidas, donde solo un conjunto específico de tipos puede implementar la interfaz.

### Ejemplo 21: Definiendo una Sealed Interface
```kotlin
// Definición de una sealed interface
sealed interface Resultado{
    data class Exito(val mensaje: String) : Resultado
    data class Error(val error: String) : Resultado
}
```

En este ejemplo, definimos una sealed interface `Resultado` y dos clases (`Exito` y `Error`) que la implementan.

### Ejemplo 22: Usando una Sealed Interface
```kotlin
// Función que maneja un Resultado
fun manejarResultado(resultado: Resultado) {
    when (resultado) {
        is Exito -> println("Éxito: ${resultado.mensaje}")
        is Error -> println("Error: ${resultado.error}")
    }
}

// Usando la sealed interface
val resultado1: Resultado = Exito("Operación completada")
val resultado2: Resultado = Error("Ocurrió un error")

manejarResultado(resultado1)
manejarResultado(resultado2)
```

Se define una función `manejarResultado` que maneja un `Resultado` usando una expresión `when`. Se crean objetos (`resultado1` y `resultado2`) de las clases que implementan la sealed interface y se pasan a la función.