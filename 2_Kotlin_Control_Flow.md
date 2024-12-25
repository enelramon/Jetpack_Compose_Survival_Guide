# Control Flow: Making Decisions and Loops

We will explore the power of conditional statements, such as `if` and `else`, and learn how to create loops to iterate through code efficiently. These constructs are the backbone of dynamic and responsive programming, enabling your Kotlin applications to adapt to changing conditions and user interactions.

## Conditional Statements: Making Informed Choices

Conditional statements allow your program to make decisions based on certain conditions. The most common form is the `if` statement, which evaluates a condition and executes a block of code if the condition is true.

### Example 1: Basic `if` Statement

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

In this example, the program assesses the temperature and prints a message accordingly. The `else if` block allows for multiple conditions, providing a more nuanced decision-making process.

### Example 2: Expressive `when` Statement

The `when` statement is a versatile alternative to the traditional `switch` statement in other languages. It evaluates multiple conditions and executes the block corresponding to the first true condition.

```kotlin
val dayOfWeek = "Wednesday"
when (dayOfWeek) {
    "Monday" -> println("It's the start of the week.")
    "Wednesday", "Thursday" -> println("Midweek vibes!")
    "Saturday" -> println("Weekend is here!")
    else -> println("Just another day.")
}
```

Here, the `when` statement checks the value of `dayOfWeek` and prints a message based on the matching condition. The `else` block serves as a fallback for any unmatched values.

## Loops: Repeating Tasks Efficiently

Loops are essential for performing repetitive tasks and iterating through collections or sequences of data. Kotlin offers both `for` and `while` loops to cater to different scenarios.

### Example 3: `for` Loop

The `for` loop is excellent for iterating through ranges, arrays, or any iterable object.

```kotlin
// Printing numbers from 1 to 5
for (i in 1..5) {
    println("Count: $i")
}
```

In this example, the `for` loop iterates through the range from 1 to 5, printing the count at each iteration. The `in` keyword simplifies the syntax, making it more readable.

### Example 4: `while` Loop

The `while` loop repeats a block of code as long as a specified condition is true.

```kotlin
var countdown = 3
while (countdown > 0) {
    println("Countdown: $countdown")
    countdown--
}
```

Here, the `while` loop creates a countdown by printing the current value of `countdown` until it reaches zero. The loop continues as long as the condition `countdown > 0` holds true.

### Example 5: `do-while` Loop

Similar to the `while` loop, the `do-while` loop executes the block of code first and then checks the condition. This ensures the block is executed at least once.

```kotlin
var attempts = 0
do {
    println("Trying to connect...")
    attempts++
} while (attempts < 3)
```

In this example, the program attempts to connect, and the loop continues until the `attempts` reach a maximum of three. This guarantees that the connection attempt is made at least once.

## Breaking and Continuing Loops

In some cases, you might want to prematurely exit a loop or skip to the next iteration. Kotlin provides the `break` and `continue` statements for these scenarios.

### Example 6: `break` Statement

The `break` statement terminates the innermost loop it is in.

```kotlin
for (i in 1..10) {
    if (i == 5) {
        println("Breaking at $i")
        break
    }
    println("Iteration: $i")
}
```

In this example, the loop iterates from 1 to 10, but when `i` reaches 5, the `break` statement is triggered, and the loop terminates.

### Example 7: `continue` Statement

The `continue` statement skips the rest of the code in the loop for the current iteration and moves to the next one.

```kotlin
for (i in 1..5) {
    if (i == 3) {
        println("Skipping iteration $i")
        continue
    }
    println("Iteration: $i")
}
```

Here, when `i` is equal to 3, the `continue` statement is executed, skipping the rest of the loop for that iteration.

## Labeling Loops

In nested loops, Kotlin allows you to label them and specify which loop to break or continue.

### Example 8: Labeled `break`

```kotlin
outerLoop@ for (i in 1..3) {
    for (j in 1..3) {
        if (i * j == 6) {
            println("Breaking at $i, $j")
            break@outerLoop
        }
        println("Iteration: $i, $j")
    }
}
```

In this example, the outer loop is labeled as `outerLoop`. When the condition `i * j == 6` is met, the `break@outerLoop` statement terminates both loops.

### Example 9: Labeled `continue`

```kotlin
outerLoop@ for (i in 1..3) {
    for (j in 1..3) {
        if (i == 2 && j == 2) {
            println("Skipping iteration $i, $j")
            continue@outerLoop
        }
        println("Iteration: $i, $j")
    }
}
```

Here, when `i` is 2 and `j` is 2, the `continue@outerLoop` statement skips the rest of the inner loop for that iteration.

## Making Choices with Control Flow

Control flow is about making choices in your program, and combining conditional statements with loops can create powerful and responsive applications. Let's explore a more complex example that integrates these concepts.

### Example 10: Dynamic Program Flow

```kotlin
val numbers = listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
for (number in numbers) {
    if (number % 2 == 0) {
        println("Even number: $number")
    } else {
        println("Odd number: $number")
    }
}
```

In this example, the program iterates through a list of numbers. For each number, it checks if it's even or odd using the modulo operator (`%`). The result is printed dynamically, showcasing the decision-making ability of control flow.

## Advanced Control Flow: `when` with Ranges and Patterns

The `when` statement in Kotlin is a powerful tool for making decisions. It's not limited to simple equality checks; you can use it with ranges and even patterns.

### Example 11: Using `when` with Ranges

```kotlin
val score = 85
when (score) {
    in 90..100 -> println("A")
    in 80 until 90 -> println("B")
    in 70 until 80 -> println("C")
    else -> println("Fail")
}
```

In this example, the program evaluates the `score` and prints the corresponding grade based on the defined ranges.

### Example 12: Using `when` with Patterns

```kotlin
val result: Any = "Success"
when (result) {
    is String -> println("Result is a String: $result")
    is Int -> println("Result is an Int: $result")
    is Boolean -> println("Result is a Boolean: $result")
    else -> println("Unknown result type")
}
```

Here, the `when` statement uses patterns to check the type of the `result` variable and prints a message accordingly. The `is` keyword is used for type checking.

## Handling Edge Cases: `try`, `catch`, and `finally`

In the real world, your programs might encounter unexpected situations or errors. Kotlin provides a robust exception handling mechanism with `try`, `catch`, and `finally` blocks.

### Example 13: Exception Handling

```kotlin
fun divide(a: Int, b: Int): Int {
    return try {
        a / b
    } catch (e: ArithmeticException) {
        println("Error: ${e.message}")
        -1
    } finally {
        println("Division attempt completed.")
    }
}
val result = divide(10, 0)
println("Result of division: $result")
```

In this example, the `divide` function attempts to perform a division. If an `ArithmeticException` occurs (such as division by zero), the `catch` block handles the exception and prints an error message. The `finally` block is executed regardless of whether an exception occurs, providing a cleanup or finalization step.

## Practical Application: Building a Simple Calculator

To tie everything together, let's build a simple calculator program using control flow constructs. This program will accept user input for two numbers and an operation, perform the calculation, and display the result.

### Example 14: Simple Calculator Program

```kotlin
fun main() {
    println("Welcome to the Simple Calculator!")
    print("Enter the first number: ")
    val num1 = readLine()?.toDoubleOrNull() ?: run {
        println("Invalid input. Exiting.")
        return
    }
    print("Enter the second number: ")
    val num2 = readLine()?.toDoubleOrNull() ?: run {
        println("Invalid input. Exiting.")
        return
    }
    print("Select the operation (+, -, *, /): ")
    val operation = readLine()
    val result = when (operation) {
        "+" -> num1 + num2
        "-" -> num1 - num2
        "*" -> num1 * num2
        "/" -> if (num2 != 0.0) num1 / num2 else "Error: Division by zero"
        else -> "Invalid operation"
    }
    println("Result: $result")
}
```

In this example, the program prompts the user for two numbers and an operation. It then uses a `when` statement to determine the appropriate calculation based on the user's choice.