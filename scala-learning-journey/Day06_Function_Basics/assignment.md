# Day 06: Practice Assignment

## Instructions
Complete the following exercises to reinforce your understanding of function basics, default parameters, named parameters, and anonymous functions. Try solving them independently before referring to the solutions.

---

## Exercise 1: Basic Function Practice

**Task:**  
Create a program with multiple functions that perform different calculations and return results.

**Requirements:**
- Create a function `calculateArea` that takes length and width, returns area
- Create a function `calculatePerimeter` that takes length and width, returns perimeter
- Create a function `celsiusToFahrenheit` that converts Celsius to Fahrenheit
- Create a function `isAdult` that takes age and returns Boolean (adult if age >= 18)
- Test all functions with sample values

**Expected Output:**
```
Rectangle (length=5, width=3):
Area: 15
Perimeter: 16

Temperature Conversion:
25°C = 77.0°F

Age Check:
Is 16 an adult? false
Is 20 an adult? true
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object BasicFunctionPractice {
  def main(args: Array[String]): Unit = {
    // Function definitions
    def calculateArea(length: Int, width: Int): Int = {
      length * width
    }
    
    def calculatePerimeter(length: Int, width: Int): Int = {
      2 * (length + width)
    }
    
    def celsiusToFahrenheit(celsius: Double): Double = {
      (celsius * 9.0 / 5.0) + 32
    }
    
    def isAdult(age: Int): Boolean = {
      age >= 18
    }
    
    // Testing functions
    val length = 5
    val width = 3
    
    println(s"Rectangle (length=$length, width=$width):")
    println(s"Area: ${calculateArea(length, width)}")
    println(s"Perimeter: ${calculatePerimeter(length, width)}")
    println()
    
    val celsius = 25.0
    println("Temperature Conversion:")
    println(f"$celsius%.0f°C = ${celsiusToFahrenheit(celsius)}%.1f°F")
    println()
    
    println("Age Check:")
    println(s"Is 16 an adult? ${isAdult(16)}")
    println(s"Is 20 an adult? ${isAdult(20)}")
  }
}
```

**Explanation:**
- Each function has clear purpose and return type
- `calculateArea` multiplies dimensions
- `calculatePerimeter` uses formula 2(l + w)
- `celsiusToFahrenheit` applies conversion formula
- `isAdult` returns Boolean based on condition
- All functions are pure (no side effects)
- Functions return values that can be used in expressions

</details>

---

## Exercise 2: Default and Named Parameters

**Task:**  
Create a user registration function with default parameters and demonstrate various calling patterns.

**Requirements:**
- Function name: `registerUser`
- Required parameter: `username` (String)
- Optional parameters with defaults:
  - `email` (default: "not-provided@example.com")
  - `age` (default: 18)
  - `country` (default: "Unknown")
  - `premium` (default: false)
- Return formatted user information string
- Test with at least 5 different calling patterns

**Expected Output:**
```
User: alice, Email: not-provided@example.com, Age: 18, Country: Unknown, Premium: false
User: bob, Email: bob@email.com, Age: 18, Country: Unknown, Premium: false
User: charlie, Email: not-provided@example.com, Age: 25, Country: Unknown, Premium: false
User: david, Email: not-provided@example.com, Age: 18, Country: USA, Premium: true
User: eve, Email: eve@email.com, Age: 30, Country: UK, Premium: true
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object DefaultParametersExample {
  def main(args: Array[String]): Unit = {
    // Function with default parameters
    def registerUser(
      username: String,
      email: String = "not-provided@example.com",
      age: Int = 18,
      country: String = "Unknown",
      premium: Boolean = false
    ): String = {
      s"User: $username, Email: $email, Age: $age, Country: $country, Premium: $premium"
    }
    
    // Different calling patterns
    
    // 1. Only required parameter
    println(registerUser("alice"))
    
    // 2. Required + one optional (positional)
    println(registerUser("bob", "bob@email.com"))
    
    // 3. Required + named parameter (skip others)
    println(registerUser("charlie", age = 25))
    
    // 4. Required + multiple named parameters
    println(registerUser("david", country = "USA", premium = true))
    
    // 5. All parameters (named for clarity)
    println(registerUser(
      username = "eve",
      email = "eve@email.com",
      age = 30,
      country = "UK",
      premium = true
    ))
  }
}
```

**Explanation:**
- Default parameters allow flexible function calls
- Callers provide only necessary information
- Named parameters enable skipping middle parameters
- Named parameters improve readability
- Order doesn't matter with named parameters
- Common pattern for configuration functions

**Real-World Application:**
```scala
// Similar to Spark configuration
def createSparkSession(
  appName: String,
  master: String = "local[*]",
  logLevel: String = "WARN",
  shufflePartitions: Int = 200
): Unit = {
  println(s"Creating Spark session: $appName")
  println(s"Master: $master")
  println(s"Log Level: $logLevel")
  println(s"Shuffle Partitions: $shufflePartitions")
}

// Usage
createSparkSession("MyApp")
createSparkSession("MyApp", master = "yarn")
createSparkSession("MyApp", logLevel = "INFO", shufflePartitions = 100)
```

</details>

---

## Exercise 3: Anonymous Functions and Collections

**Task:**  
Create a program that uses anonymous functions to transform and filter a collection of numbers.

**Requirements:**
- Start with a list of numbers: 1 to 20
- Use anonymous functions (lambdas) to:
  1. Double each number
  2. Filter numbers greater than 15
  3. Square each number
  4. Calculate the sum of all numbers
- Print results of each operation
- Show both full lambda syntax and underscore shortcut

**Expected Output:**
```
Original: List(1, 2, 3, ... 20)
Doubled: List(2, 4, 6, ... 40)
Greater than 15: List(16, 17, 18, 19, 20)
Squared: List(1, 4, 9, ... 400)
Sum: 210
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object AnonymousFunctionsPractice {
  def main(args: Array[String]): Unit = {
    val numbers = (1 to 20).toList
    
    println(s"Original: $numbers")
    println()
    
    // 1. Double each number - full lambda syntax
    val doubled = numbers.map((x: Int) => x * 2)
    println(s"Doubled (full syntax): $doubled")
    
    // Same with underscore shortcut
    val doubledShort = numbers.map(_ * 2)
    println(s"Doubled (shortcut): $doubledShort")
    println()
    
    // 2. Filter numbers greater than 15
    val filtered = numbers.filter((x: Int) => x > 15)
    println(s"Greater than 15 (full): $filtered")
    
    val filteredShort = numbers.filter(_ > 15)
    println(s"Greater than 15 (shortcut): $filteredShort")
    println()
    
    // 3. Square each number
    val squared = numbers.map((x: Int) => x * x)
    println(s"Squared (full): $squared")
    
    val squaredShort = numbers.map(x => x * x)  // Can't use _ * _ here
    println(s"Squared (named): $squaredShort")
    println()
    
    // 4. Calculate sum
    val sum = numbers.reduce((a: Int, b: Int) => a + b)
    println(s"Sum (full): $sum")
    
    val sumShort = numbers.reduce(_ + _)
    println(s"Sum (shortcut): $sumShort")
    
    // Complex chaining
    val result = numbers
      .filter(_ % 2 == 0)   // Keep evens
      .map(_ * 3)           // Triple them
      .filter(_ > 20)       // Keep > 20
    
    println()
    println(s"Complex chain (evens * 3 > 20): $result")
  }
}
```

**Explanation:**
- Anonymous functions defined inline without names
- `map` transforms each element
- `filter` selects elements matching condition
- `reduce` combines elements into single value
- Underscore `_` shortcut for simple operations
- Cannot use `_` when parameter appears multiple times (like `x * x`)
- Chaining operations creates data transformation pipelines
- Each operation returns new collection (immutable)

**Why This Matters:**
```scala
// This pattern is fundamental in Spark
val data = spark.read.csv("data.csv")
val result = data
  .filter($"age" > 18)        // Like filter(_ > 18)
  .map(row => transform(row)) // Like map with custom function
  .reduce(_ + _)              // Aggregate results
```

</details>

---

## Exercise 4: Building a Simple Calculator

**Task:**  
Create a calculator program using functions with different parameter types.

**Requirements:**
- Create functions for: add, subtract, multiply, divide
- Create a `calculate` function that takes operation name and two numbers
- Use pattern matching in `calculate` to call appropriate function
- Handle division by zero
- Create anonymous functions for percentage and power calculations
- Test all operations

**Expected Output:**
```
10 + 5 = 15
10 - 5 = 5
10 * 5 = 50
10 / 5 = 2.0
10 / 0 = Error: Division by zero
20% of 150 = 30.0
2^8 = 256.0
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object CalculatorProgram {
  def main(args: Array[String]): Unit = {
    // Basic operation functions
    def add(a: Double, b: Double): Double = a + b
    def subtract(a: Double, b: Double): Double = a - b
    def multiply(a: Double, b: Double): Double = a * b
    def divide(a: Double, b: Double): Double = {
      if (b != 0) a / b
      else {
        println("Error: Division by zero")
        0.0
      }
    }
    
    // Master calculator function
    def calculate(operation: String, a: Double, b: Double): Double = {
      operation match {
        case "add" => add(a, b)
        case "subtract" => subtract(a, b)
        case "multiply" => multiply(a, b)
        case "divide" => divide(a, b)
        case _ => {
          println(s"Unknown operation: $operation")
          0.0
        }
      }
    }
    
    // Anonymous functions for advanced operations
    val percentage = (percent: Double, value: Double) => (percent / 100) * value
    val power = (base: Double, exponent: Double) => Math.pow(base, exponent)
    
    // Testing basic operations
    println(s"10 + 5 = ${calculate("add", 10, 5)}")
    println(s"10 - 5 = ${calculate("subtract", 10, 5)}")
    println(s"10 * 5 = ${calculate("multiply", 10, 5)}")
    println(s"10 / 5 = ${calculate("divide", 10, 5)}")
    println(s"10 / 0 = ${calculate("divide", 10, 0)}")
    
    // Testing anonymous functions
    println(s"20% of 150 = ${percentage(20, 150)}")
    println(s"2^8 = ${power(2, 8)}")
  }
}
```

**Explanation:**
- Named functions for basic operations (add, subtract, etc.)
- `calculate` function acts as dispatcher using pattern matching
- Division function includes error handling
- Anonymous functions for specialized calculations
- Functions return values (no side effects except error messages)
- Pattern matching provides clean operation selection

**Enhanced Version with Better Error Handling:**
```scala
def safeDivide(a: Double, b: Double): Option[Double] = {
  if (b != 0) Some(a / b)
  else None
}

// Usage
safeDivide(10, 2) match {
  case Some(result) => println(s"Result: $result")
  case None => println("Cannot divide by zero")
}
```

</details>

---

## Bonus Challenge (Optional)

**Task:**  
Create a text processing utility with multiple functions that use anonymous functions and underscore shortcuts.

**Requirements:**
- Create function `processWords` that takes a string and a list of transformations
- Transformations should be anonymous functions that modify strings
- Apply all transformations in sequence
- Provide transformations for: uppercase, reverse, add prefix, add suffix
- Test with sample sentences

**Expected Output:**
```
Original: hello world
Uppercase: HELLO WORLD
Reversed: dlrow olleh
With prefix: [hello world]
With suffix: hello world!
All transforms: [!DLROW OLLEH]
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object TextProcessingChallenge {
  def main(args: Array[String]): Unit = {
    // Function that applies list of transformations
    def processWords(
      text: String,
      transformations: List[String => String]
    ): String = {
      transformations.foldLeft(text)((current, transform) => transform(current))
    }
    
    // Define transformations as anonymous functions
    val toUpperCase = (s: String) => s.toUpperCase()
    val reverse = (s: String) => s.reverse
    val addPrefix = (s: String) => s"[$s]"
    val addSuffix = (s: String) => s"$s!"
    
    // Test text
    val originalText = "hello world"
    
    println(s"Original: $originalText")
    println(s"Uppercase: ${processWords(originalText, List(toUpperCase))}")
    println(s"Reversed: ${processWords(originalText, List(reverse))}")
    println(s"With prefix: ${processWords(originalText, List(addPrefix))}")
    println(s"With suffix: ${processWords(originalText, List(addSuffix))}")
    
    // Apply all transformations
    val allTransforms = List(toUpperCase, reverse, addPrefix, addSuffix)
    println(s"All transforms: ${processWords(originalText, allTransforms)}")
    
    // Using inline anonymous functions
    val customProcess = processWords(
      "scala programming",
      List(
        _.toUpperCase(),
        _.replace(" ", "_"),
        s => s"***$s***"
      )
    )
    println(s"Custom process: $customProcess")
  }
}
```

**Explanation:**
- `processWords` takes text and list of transformation functions
- `foldLeft` applies each transformation sequentially
- Anonymous functions define transformations
- Transformations are first-class values (can be stored in variables)
- Transformations compose (output of one becomes input of next)
- Underscore shortcut used in inline transformations

**Key Concept - Function Composition:**
```scala
// Each transformation takes String, returns String
// They can be chained together
val result = addSuffix(addPrefix(reverse(toUpperCase("hello"))))
// Same as:
val result = "hello"
  .toUpperCase()  // "HELLO"
  .reverse        // "OLLEH"
  // (addPrefix)  // "[OLLEH]"
  // (addSuffix)  // "[OLLEH]!"
```

</details>

---

## Testing Your Programs

### Run and Verify:

1. **IntelliJ IDEA:**
   - Create new Scala object
   - Paste code
   - Right-click → Run
   - Verify output matches expected

2. **Experiment:**
   - Change function parameters
   - Add new functions
   - Try different anonymous function patterns
   - Test edge cases (zero, negative numbers, empty strings)

---

## Self-Assessment Checklist

After completing these exercises, you should be able to:
- [ ] Define functions with `def` keyword
- [ ] Specify parameter types and return types
- [ ] Use automatic return (no `return` keyword)
- [ ] Create functions with default parameters
- [ ] Call functions with named parameters
- [ ] Skip middle parameters using named arguments
- [ ] Write anonymous functions (lambdas)
- [ ] Use underscore shortcut for simple operations
- [ ] Apply functions to collections (map, filter, reduce)
- [ ] Chain multiple function operations
- [ ] Understand when to use explicit vs. shortcut syntax

---

## Common Mistakes to Avoid

1. **Forgetting parameter types:**
```scala
   // Wrong
   // def add(a, b) = a + b
   
   // Correct
   def add(a: Int, b: Int) = a + b
```

2. **Using `return` keyword:**
```scala
   // Scala style (preferred)
   def add(a: Int, b: Int) = a + b
   
   // Java style (unnecessary)
   def add(a: Int, b: Int) = {
     return a + b  // Don't do this
   }
```

3. **Misusing underscore:**
```scala
   // Wrong - parameter used twice
   // numbers.map(_ * _)  // Treats as two parameters
   
   // Correct
   numbers.map(x => x * x)
```

4. **Named after positional:**
```scala
   // Wrong order
   // register("alice@email.com", username = "alice")
   
   // Correct - positional first
   register("alice", email = "alice@email.com")
```

---

## Key Concepts Review

**Remember:**
- Functions encapsulate reusable logic
- Parameter types required, return type optional but recommended
- Default parameters provide flexibility
- Named parameters improve readability
- Anonymous functions for inline operations
- Underscore `_` shortcut for simple transformations
- Functions are expressions (return values)

---

**Next Steps:** Once you've completed these exercises, you're ready for Day 07 where we'll learn about advanced functional concepts including higher-order functions, closures, and currying!

---