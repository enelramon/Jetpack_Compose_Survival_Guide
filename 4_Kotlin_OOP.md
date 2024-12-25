# Object Oriented Programming
Object-Oriented Programming revolves around the concept of objects, which are instances of classes, and Kotlin seamlessly integrates these concepts into its syntax. We'll explore classes, objects, inheritance, polymorphism, encapsulation, and more. Let's dive into the fundamentals of OOP and how they manifest in Kotlin.

## Classes and Objects: The Building Blocks of OOP
At the core of Object-Oriented Programming are classes and objects. A class is a blueprint or template that defines the structure and behavior of objects. Objects, on the other hand, are instances of classes, representing real-world entities. Kotlin simplifies the creation of classes and objects, making it an ideal language for OOP.

### Example 1: Creating a Simple Class
```kotlin
// DefinCar {
    var brand: String = ""
    var model: String = ""
    var year: Int = 0

    fun displayInfo() {
        println("Car: $brand $model, Year: $year")
    }
}
```

In this example, we define a `Car` class with properties (`brand`, `model`, `year`) and a method (`displayInfo`) to showcase information about the car.

### Example 2: Creating Objects from a Class
```kotlin
// Creating objects of the Car class
val car1 = Car()
car1.brand = "Toyota"
car1.model = "Camry"
car1.year = 2022

val car2 = Car()
car2.brand = "Honda"
car2.model = "Civic"
car2.year = 2021

// Calling the displayInfo method on each car object
car1.displayInfo()
car2.displayInfo()
```

Here, we create two objects (`car1` and `car2`) from the `Car` class and set their properties. The `displayInfo` method is then called on each object to showcase their information.

## Constructors: Initializing Objects with Ease
Constructors are special methods in a class that are responsible for initializing the properties of an object when it is created. Kotlin offers concise and flexible ways to define constructors.

### Example 3: Primary Constructor
```kotlin
// Class with a primary constructor
class Book(val title: String, val author: String, val year: Int) {
    // Method to display information about the book
    fun displayInfo() {
        println("Book: $title by $author, Year: $year")
    }
}
```

In this example, we define a `Book` class with a primary constructor. The properties (`title`, `author`, `year`) are declared directly in the constructor header.

### Example 4: Creating Objects with Primary Constructor
```kotlin
// Creating objects using the primary constructor
val book1 = Book("The Alchemist", "Paulo Coelho", 1988)
val book2 = Book("To Kill a Mockingbird", "Harper Lee", 1960)

// Calling the displayInfo method on each book object
book1.displayInfo()
book2.displayInfo()
```

Objects (`book1` and `book2`) are created using the primary constructor, providing values for the properties. The `displayInfo` method is then called to showcase information about each book.

### Example 5: Secondary Constructor
```kotlin
class Movie {
    var title: String = ""
    var director: String = ""
    var year: Int = 0

    // Secondary constructor with additional parameters
    constructor(title: String, director: String, year: Int) {
        this.title = title
        this.director = director
        this.year = year
    }

    // Method to display information about the movie
    fun displayInfo() {
        println("Movie: $title directed by $director, Year: $year")
    }
}
```

Here, we define a `Movie` class with a secondary constructor that allows additional parameters to be passed when creating an object.

### Example 6: Creating Objects with Secondary Constructor
```kotlin
// Creating objects using the secondary constructor
val movie1 = Movie("Inception", "Christopher Nolan", 2010)
val movie2 = Movie("The Shawshank Redemption", "Frank Darabont", 1994)

// Calling the displayInfo method on each movie object
movie1.displayInfo()
movie2.displayInfo()
```

Objects (`movie1` and `movie2`) are created using the secondary constructor, providing values for the additional parameters. The `displayInfo` method showcases information about each movie.

## Inheritance: Building Hierarchies of Classes
Inheritance is a fundamental concept in OOP that allows a class to inherit properties and behaviors from another class. Kotlin supports single-class inheritance, providing a clean and structured way to build class hierarchies.

### Example 7: Inheriting from a Base Class
```kotlin
// Base class
open class Animal(val name: String) {
    open fun makeSound() {
        println("Animal $name makes a generic sound")
    }
}

// Derived class inheriting from Animal
class Dog(name: String) : Animal(name) {
    // Overriding the makeSound method
    override fun makeSound() {
        println("Dog $name barks loudly")
    }
}
```

In this example, we define a base class (`Animal`) with a property (`name`) and a method (`makeSound`). The `Dog` class inherits from `Animal` and overrides the `makeSound` method.

### Example 8: Using Inherited Classes
```kotlin
// Creating objects of the base and derived classes
val genericAnimal = Animal("Generic")
val fluffyDog = Dog("Fluffy")

// Calling the makeSound method on each object
genericAnimal.makeSound()
fluffyDog.makeSound()
```

Objects (`genericAnimal` and `fluffyDog`) are created from the base and derived classes. The `makeSound` method is called on each object, demonstrating polymorphism.

## Polymorphism: Embracing Diversity in Types
Polymorphism allows objects of different types to be treated as objects of a common base type. Kotlin supports polymorphism through class inheritance and interface implementation.

### Example 9: Using Polymorphism with Interfaces
```kotlin
// Interface defining the sound behavior
interface SoundMaker {
    fun makeSound()
}

// Class implementing the SoundMaker interface
class Cat(val name: String) : SoundMaker {
    // Implementing the makeSound method
    override fun makeSound() {
        println("Cat $name purrs softly")
    }
}
```

Here, we define an `SoundMaker` interface with a `makeSound` method. The `Cat` class implements this interface.

### Example 10: Using Polymorphism with Interface Objects
```kotlin
// Creating objects of the interface and implementing class
val soundMaker: SoundMaker = Cat("Whiskers")

// Calling the makeSound method on the interface object
soundMaker.makeSound()
```

An object (`soundMaker`) of the `SoundMaker` interface is created, pointing to a `Cat` instance. The `makeSound` method is called on the interface object, showcasing polymorphism.

## Encapsulation: Protecting the Inside World
Encapsulation is the practice of bundling the data (properties) and methods that operate on the data within a single unit, known as a class. Kotlin provides visibility modifiers to control the accessibility of properties and methods.

### Example 11: Using Visibility Modifiers
```kotlin
// Class with private and public properties
class BankAccount {
    private var balance: Double = 0.0

    // Public method to deposit money
    fun deposit(amount: Double) {
        if (amount > 0) {
            balance += amount
            println("Deposit successful. New balance: $balance")
        } else {
            println("Invalid deposit amount")
        }
    }

    // Public method to check balance
    fun checkBalance() {
        println("Current balance: $balance")
    }
}
```

In this example, the `BankAccount` class has a private property (`balance`) and public methods (`deposit`, `checkBalance`). The `balance` is encapsulated, and its access is restricted to methods within the class.

### Example 12: Interacting with Encapsulated Class
```kotlin
// Creating an object of the BankAccount class
val account = BankAccount()

// Performing operations on the object
account.deposit(1000.0)
account.deposit(-500.0) // Invalid deposit amount
account.checkBalance()
```

An object (`account`) of the `BankAccount` class is created, and operations like depositing money and checking the balance are performed. The encapsulation ensures that the `balance` property is accessed and modified only through the class methods.

## Abstract Classes and Interfaces: Blueprint for Types
Abstract classes and interfaces provide blueprints for other classes to follow. Abstract classes can have both abstract and concrete methods, while interfaces only declare abstract methods.

### Example 13: Abstract Class
```kotlin
// Abstract class with abstract and concrete methods
abstract class Shape(val name: String) {
    // Abstract method to calculate area
    abstract fun calculateArea(): Double

    // Concrete method
    fun displayInfo() {
        println("$name - Area: ${calculateArea()}")
    }
}
```

Here, we define an abstract class (`Shape`) with an abstract method (`calculateArea`) and a concrete method (`displayInfo`).

### Example 14: Concrete Class Extending Abstract Class
```kotlin
// Concrete class extending the abstract class
class Circle(radius: Double) : Shape("Circle") {
    // Overriding the abstract method to calculate area
    override fun calculateArea(): Double {
        return 3.14 * radius * radius
    }
}
```

The `Circle` class extends the `Shape` abstract class and provides an implementation for the abstract method.

### Example 15: Using Abstract Class and Concrete Class
```kotlin
// Creating an object of the concrete class
val circle = Circle(5.0)

// Calling the concrete and abstract methods
circle.displayInfo()
```

An object (`circle`) of the `Circle` class is created, and the `displayInfo` method is called, showcasing the use of abstract and concrete methods.

### Example 16: Interface
```kotlin
// Interface defining the behavior of a printer
interface Printer {
    fun print(document: String)
}
```

Here, we define a `Printer` interface with a single abstract method (`print`).

### Example 17: Class Implementing an Interface
```kotlin
// Class implementing the Printer interface
class LaserPrinter : Printer {
    // Implementing the print method
    override fun print(document: String) {
        println("Printing document: $document (Laser Printer)")
    }
}
```

The `LaserPrinter` class implements the `Printer` interface and provides an implementation for the `print` method.

### Example 18: Using Interface and Implementing Class
```kotlin
// Creating an object of the implementing class
val laserPrinter = LaserPrinter()

// Calling the interface method through the implementing class
laserPrinter.print("Sales Report")
```

An object (`laserPrinter`) of the `LaserPrinter` class is created, and the `print` method is called, demonstrating the use of interfaces.