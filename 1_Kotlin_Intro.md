# Kotlin Basics: Variables, Data Types, and Operators
Now that your development environment is set up, let's delve into the building blocks of Kotlin: variables, data types, and operators.

In Kotlin, declaring a variable is a breeze. The `val` keyword is used for read-only variables, and `var` for mutable ones.

### Example 1: Declaring Variables
```kotlin
val pi = 3.14
var counter = 0
```

In this example, `pi` is a read-only variable initialized with the value 3.14, while `counter` is a mutable variable starting at 0.

### Data Types in Kotlin
Kotlin supports a rich set of data types, including integers, floats, booleans, and strings.
The compiler automatically infers the type, but you can also explicitly specify it.

### Example 2: Exploring Data Types
```kotlin
val age: Int = 25
val height: Double = 175.5
val isStudent: Boolean = true
val name: String = "Alice"
```

Here, we've defined variables with specific data types: `age` as an integer, `height` as a double, `isStudent` as a boolean, and `name` as a string.

### Operators in Kotlin
Kotlin inherits familiar operators from Java but introduces some improvements.
Let's explore a few.

### Example 3: Mathematical Operators
```kotlin
val x = 10
val y = 5
val sum = x + y
val difference = x - y
val product = x * y
val quotient = x / y
val remainder = x % y
```

### Example 4: String Concatenation
Kotlin makes string manipulation intuitive.
```kotlin
val firstName = "John"
val lastName = "Doe"
val fullName = firstName + " " + lastName
```

Here, the `+` operator concatenates the first name, a space, and the last name to create the full name.

### Example 5: Comparison Operators
Comparing values is a common operation in programming.
```kotlin
val a = 10
val b = 20
val isEqual = (a == b)
val isNotEqual = (a != b)
val isGreater = (a > b)
val isLessOrEqual = (a <= b)
```
These operators evaluate conditions and return a boolean result, aiding in decision-making within your code.

## Control Flow: Making Decisions and Loops
Now that you're familiar with variables, data types, and operators, let's explore control flow. This involves making decisions and looping through code, essential for creating dynamic and responsive programs.

### Example 6: Conditional Statements (if-else)
```kotlin
val temperature = 25
if (temperature > 30) {
    println("It's a hot day!")
} else if (temperature in 20..30) {
    println("The weather is pleasant.")
} else {
    println("It's a bit chilly.")
}
```

In this example, the program decides what to print based on the value of the `temperature` variable. The `in` keyword is used to check if the temperature falls within a specific range.

### Example 7: Loops (for and while)
```kotlin
// For loop
for (i in 1..5) {
    println("Count: $i")
}

// While loop
var countdown = 3
while (countdown > 0) {
    println("Countdown: $countdown")
    countdown--
}
```

Here, a `for` loop counts from 1 to 5, printing the count at each iteration. The `while` loop creates a countdown, printing the current value until it reaches zero.

## Functions in Kotlin: Defining and Calling

Functions are blocks of reusable code that perform a specific task. They help in organizing your code and avoiding redundancy.

### Example 8: Defining and Calling Functions

```kotlin
fun greet(name: String) {
    println("Hello, $name!")
}

// Calling the function
greet("Bob")
```

Here, we define a function `greet` that takes a `name` parameter and prints a personalized greeting. Calling the function with the argument "Bob" produces the output "Hello, Bob!"

### Example 9: Returning Values
```kotlin
fun square(number: Int): Int {
    return number * number
}

val result = square(5)
println("Square of 5 is: $result")
```

The `square` function takes an integer `number` as a parameter and returns its square.

## Object-Oriented Programming with Kotlin

Kotlin is a fully object-oriented language, allowing you to structure your code using classes and objects.

### Example 10: Creating a Simple Class

```kotlin
class Car(val model: String, val year: Int) {
    fun startEngine() {
        println("Engine started for $model")
    }

    fun drive() {
        println("$model is on the move!")
    }
}

// Creating an instance of the Car class
val myCar = Car("Tesla", 2022)

// Accessing properties and calling methods
println("My car is a ${myCar.model} from ${myCar.year}")
myCar.startEngine()
myCar.drive()
```
Here, we define a `Car` class with properties `model` and `year`, along with methods to start the engine and drive. We then create an instance of the class (`myCar`) and interact with it.

## Exception Handling and Error Management
In the real world, errors happen. Kotlin provides a robust system for handling exceptions gracefully.
### Example 11: Handling Exceptions
```kotlin
fun divide(a: Int, b: Int): Int {
    return try {
        a / b
    } catch (e: ArithmeticException) {
        println("Error: ${e.message}")
        -1
    }
}
val result = divide(10, 0)
println("Result of division: $result")
```
The `divide` function attempts to perform a division and catches any `ArithmeticException`. In case of an error, it prints a message and returns -1.

## Collections in Kotlin: Lists, Maps, and Sets
Collections are containers that hold multiple items. Kotlin provides a rich set of collection types, including lists, maps, and sets.
### Example 12: Working with Lists
```kotlin
val fruits = listOf("Apple", "Banana", "Orange")
// Accessing elements
val firstFruit = fruits[0]
// Iterating through the list
for (fruit in fruits) {
    println(fruit)
}
```
Here, `fruits` is a list of strings. We access the first element using index notation and then iterate through the list, printing each fruit.

### Example 13: Maps and Sets
```kotlin
val capitals = mapOf("USA" to "Washington, D.C.", "France" to "Paris", "Japan" to "Tokyo")
// Accessing values
val capitalOfUSA = capitals["USA"]
// Working with sets
val uniqueNumbers = setOf(1, 2, 3, 4, 5, 1, 2)
// Iterating through the set
for (number in uniqueNumbers) {
    println(number)
}
```
In this example, `capitals` is a map representing country-capital pairs, and `uniqueNumbers` is a set with no duplicate values.