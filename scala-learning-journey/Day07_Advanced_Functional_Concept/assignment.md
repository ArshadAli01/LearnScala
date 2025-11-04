# Day 07: Practice Assignment

## Instructions
Complete the following exercises to reinforce your understanding of higher-order functions, closures, lazy evaluation, partial application, and currying. Try solving them independently before referring to the solutions.

---

## Exercise 1: Higher-Order Functions

**Task:**  
Create a calculator program that uses higher-order functions to process operations.

**Requirements:**
- Create a function `calculate` that accepts two numbers and an operation function
- Define at least 5 different operations (add, subtract, multiply, divide, modulo)
- Create a function `applyToList` that applies an operation to all pairs in a list
- Test with various operations

**Expected Output:**
```
5 + 3 = 8
5 - 3 = 2
5 * 3 = 15
5 / 3 = 1.6666666666666667
5 % 3 = 2

Applying operations to list:
List((2, 1), (4, 2), (6, 3))
Additions: List(3, 6, 9)
Multiplications: List(2, 8, 18)
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object HigherOrderCalculator {
  def main(args: Array[String]): Unit = {
    // Higher-order function accepting operation
    def calculate(a: Double, b: Double, operation: (Double, Double) => Double): Double = {
      operation(a, b)
    }
    
    // Define operations as functions
    val add = (x: Double, y: Double) => x + y
    val subtract = (x: Double, y: Double) => x - y
    val multiply = (x: Double, y: Double) => x * y
    val divide = (x: Double, y: Double) => x / y
    val modulo = (x: Double, y: Double) => x % y
    
    // Test basic operations
    println(s"5 + 3 = ${calculate(5, 3, add)}")
    println(s"5 - 3 = ${calculate(5, 3, subtract)}")
    println(s"5 * 3 = ${calculate(5, 3, multiply)}")
    println(s"5 / 3 = ${calculate(5, 3, divide)}")
    println(s"5 % 3 = ${calculate(5, 3, modulo)}")
    println()
    
    // Higher-order function for list processing
    def applyToList(
      pairs: List[(Double, Double)],
      operation: (Double, Double) => Double
    ): List[Double] = {
      pairs.map { case (a, b) => operation(a, b) }
    }
    
    val numberPairs = List((2.0, 1.0), (4.0, 2.0), (6.0, 3.0))
    
    println("Applying operations to list:")
    println(numberPairs)
    println(s"Additions: ${applyToList(numberPairs, add)}")
    println(s"Multiplications: ${applyToList(numberPairs, multiply)}")
  }
}
```

**Explanation:**
- `calculate` is a higher-order function accepting operation function
- Operations defined as function values
- `applyToList` demonstrates applying operation to collection
- Pattern matching extracts tuple values
- Same higher-order function works with different operations
- This pattern common in functional programming and Spark

</details>

---

## Exercise 2: Closures and State

**Task:**  
Create a banking system using closures to encapsulate account state.

**Requirements:**
- Create function `createAccount` that returns account operations
- Account should track balance (private state in closure)
- Return functions for: deposit, withdraw, checkBalance
- Each account should maintain independent state
- Handle insufficient funds for withdrawal

**Expected Output:**
```
Account 1:
Deposited: 1000.0, New balance: 1000.0
Withdrew: 300.0, New balance: 700.0
Current balance: 700.0
Insufficient funds. Current balance: 700.0
Current balance: 700.0

Account 2:
Deposited: 500.0, New balance: 500.0
Current balance: 500.0
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object ClosureBankingSystem {
  def main(args: Array[String]): Unit = {
    // Closure maintaining private state
    def createAccount(initialBalance: Double): (String, Double) => Unit = {
      var balance = initialBalance
      
      (operation: String, amount: Double) => {
        operation match {
          case "deposit" =>
            balance += amount
            println(f"Deposited: $amount%.1f, New balance: $balance%.1f")
          
          case "withdraw" =>
            if (balance >= amount) {
              balance -= amount
              println(f"Withdrew: $amount%.1f, New balance: $balance%.1f")
            } else {
              println(f"Insufficient funds. Current balance: $balance%.1f")
            }
          
          case "balance" =>
            println(f"Current balance: $balance%.1f")
          
          case _ =>
            println("Invalid operation")
        }
      }
    }
    
    // Create independent accounts
    val account1 = createAccount(0)
    val account2 = createAccount(0)
    
    println("Account 1:")
    account1("deposit", 1000)
    account1("withdraw", 300)
    account1("balance", 0)
    account1("withdraw", 800)  // Should fail
    account1("balance", 0)
    println()
    
    println("Account 2:")
    account2("deposit", 500)
    account2("balance", 0)
  }
}
```

**Explanation:**
- `createAccount` returns function that closes over `balance`
- `balance` is mutable but private (encapsulated)
- Each account maintains independent state
- Closure provides encapsulation without classes
- Pattern matching handles different operations
- State persists between function calls

**Alternative with Better API:**
```scala
case class AccountOperations(
  deposit: Double => Unit,
  withdraw: Double => Unit,
  checkBalance: () => Unit
)

def createAccountV2(initialBalance: Double): AccountOperations = {
  var balance = initialBalance
  
  AccountOperations(
    deposit = (amount: Double) => {
      balance += amount
      println(f"Deposited: $amount%.1f, Balance: $balance%.1f")
    },
    withdraw = (amount: Double) => {
      if (balance >= amount) {
        balance -= amount
        println(f"Withdrew: $amount%.1f, Balance: $balance%.1f")
      } else {
        println(f"Insufficient funds: $balance%.1f")
      }
    },
    checkBalance = () => {
      println(f"Balance: $balance%.1f")
    }
  )
}

// Usage
val account = createAccountV2(1000)
account.deposit(500)
account.withdraw(300)
account.checkBalance()
```

</details>

---

## Exercise 3: By-Name Parameters and Lazy Evaluation

**Task:**  
Create a logging system that only computes log messages when needed using by-name parameters.

**Requirements:**
- Create function `log` with log level, enabled flag, and by-name message
- Only compute message if logging is enabled for that level
- Create expensive computation function to demonstrate lazy evaluation
- Test with enabled and disabled logging
- Measure execution time difference

**Expected Output:**
```
[INFO] Application started
Computing expensive message...
[DEBUG] Expensive debug info: Result after 1000ms
[ERROR] Critical error occurred

Disabled logging test:
(No expensive computation output)
All done!
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object LazyLoggingSystem {
  def main(args: Array[String]): Unit = {
    // Log levels
    var enableDebug = true
    var enableInfo = true
    
    // By-name parameter for lazy message evaluation
    def log(level: String, enabled: Boolean, message: => String): Unit = {
      if (enabled) {
        println(s"[$level] $message")
      }
      // If not enabled, message is NEVER evaluated
    }
    
    // Expensive computation
    def expensiveComputation(): String = {
      println("Computing expensive message...")
      Thread.sleep(1000)
      s"Result after 1000ms"
    }
    
    // Test logging
    log("INFO", enableInfo, "Application started")
    log("DEBUG", enableDebug, s"Expensive debug info: ${expensiveComputation()}")
    log("ERROR", true, "Critical error occurred")
    println()
    
    // Disable debug logging
    println("Disabled logging test:")
    enableDebug = false
    log("DEBUG", enableDebug, expensiveComputation())  // Never computed!
    println("All done!")
    
    // Timing demonstration
    println("\nTiming comparison:")
    
    val start1 = System.currentTimeMillis()
    log("DEBUG", false, expensiveComputation())
    val end1 = System.currentTimeMillis()
    println(s"Disabled logging time: ${end1 - start1}ms")  // ~0ms
    
    val start2 = System.currentTimeMillis()
    log("DEBUG", true, expensiveComputation())
    val end2 = System.currentTimeMillis()
    println(s"Enabled logging time: ${end2 - start2}ms")  // ~1000ms
  }
}
```

**Explanation:**
- `message: => String` is by-name parameter
- Message only evaluated if `enabled` is true
- Expensive computation avoided when logging disabled
- Demonstrates performance benefit of lazy evaluation
- Common pattern in logging frameworks

**Real-World Pattern:**
```scala
// Timing utility using by-name parameters
def measureTime[A](operation: => A): (A, Long) = {
  val start = System.currentTimeMillis()
  val result = operation  // Executed here
  val end = System.currentTimeMillis()
  (result, end - start)
}

// Usage
val (result, time) = measureTime {
  // Any expensive operation
  (1 to 1000000).sum
}
println(s"Result: $result, Time: ${time}ms")
```

</details>

---

## Exercise 4: Currying and Partial Application

**Task:**  
Create a text formatting system using currying to build specialized formatters.

**Requirements:**
- Create curried function `formatText` with prefix, suffix, and content parameters
- Use partial application to create specialized formatters:
  - HTML bold, italic, underline
  - Markdown bold, italic, code
  - Brackets, parentheses, quotes
- Test all formatters with sample text

**Expected Output:**
```
HTML Formatters:
<b>Important</b>
<i>Emphasis</i>
<u>Underlined</u>

Markdown Formatters:
**Bold Text**
*Italic Text*
`Code Block`

Delimiters:
[Bracketed]
(Parenthesized)
"Quoted"
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object CurryingFormatters {
  def main(args: Array[String]): Unit = {
    // Curried function for text formatting
    def formatText(prefix: String)(suffix: String)(content: String): String = {
      s"$prefix$content$suffix"
    }
    
    // HTML formatters using partial application
    val htmlBold = formatText("<b>")("</b>") _
    val htmlItalic = formatText("<i>")("</i>") _
    val htmlUnderline = formatText("<u>")("</u>") _
    
    println("HTML Formatters:")
    println(htmlBold("Important"))
    println(htmlItalic("Emphasis"))
    println(htmlUnderline("Underlined"))
    println()
    
    // Markdown formatters
    val mdBold = formatText("**")("**") _
    val mdItalic = formatText("*")("*") _
    val mdCode = formatText("`")("`") _
    
    println("Markdown Formatters:")
    println(mdBold("Bold Text"))
    println(mdItalic("Italic Text"))
    println(mdCode("Code Block"))
    println()
    
    // Delimiter formatters
    val brackets = formatText("[")("]") _
    val parentheses = formatText("(")(")" _
    val quotes = formatText("\"")("""") _
    
    println("Delimiters:")
    println(brackets("Bracketed"))
    println(parentheses("Parenthesized"))
    println(quotes("Quoted"))
    
    // Complex formatter combining multiple
    def multiFormat(text: String): String = {
      htmlBold(htmlItalic(text))
    }
    
    println("\nCombined:")
    println(multiFormat("Bold and Italic"))
  }
}
```

**Explanation:**
- `formatText` is curried (three parameter groups)
- Partial application creates specialized formatters
- Underscore `_` converts to partially applied function
- Each formatter "remembers" its prefix and suffix
- Can compose formatters for complex formatting
- Demonstrates code reuse through currying

**Advanced Example with Configuration:**
```scala
// Curried function for data processing configuration
def processData(format: String)(compressionLevel: Int)(partitions: Int)(data: List[String]): Unit = {
  println(s"Format: $format")
  println(s"Compression: $compressionLevel")
  println(s"Partitions: $partitions")
  println(s"Processing ${data.size} records")
}

// Create configured processors
val parquetProcessor = processData("parquet") _
val parquetHighCompression = parquetProcessor(9) _
val parquet100Partitions = parquetHighCompression(100) _

// Apply to data
val data = List("record1", "record2", "record3")
parquet100Partitions(data)

// Or configure everything at once
val csvProcessor = processData("csv")(5)(50) _
csvProcessor(data)
```

</details>

---

## Bonus Challenge (Optional)

**Task:**  
Create a function pipeline builder that allows chaining transformations using higher-order functions and currying.

**Requirements:**
- Create function `pipeline` that accepts initial value
- Allow chaining transformations with `.then` method
- Create at least 5 different transformations
- Support both value transformations and side effects
- Test with a complex pipeline

**Expected Output:**
```
Starting pipeline with: 5
After double: 10
After addTen: 20
After square: 400
After toString: 400
After uppercase: 400
Final result: 400
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object PipelineBuilder {
  def main(args: Array[String]): Unit = {
    // Pipeline builder class using higher-order functions
    case class Pipeline[A](value: A) {
      def then[B](transform: A => B): Pipeline[B] = {
        Pipeline(transform(value))
      }
      
      def tap(sideEffect: A => Unit): Pipeline[A] = {
        sideEffect(value)
        this
      }
      
      def get: A = value
    }
    
    // Create transformations
    val double = (x: Int) => x * 2
    val addTen = (x: Int) => x + 10
    val square = (x: Int) => x * x
    val toString = (x: Int) => x.toString
    val uppercase = (s: String) => s.toUpperCase
    
    // Build and execute pipeline
    val result = Pipeline(5)
      .tap(x => println(s"Starting pipeline with: $x"))
      .then(double)
      .tap(x => println(s"After double: $x"))
      .then(addTen)
      .tap(x => println(s"After addTen: $x"))
      .then(square)
      .tap(x => println(s"After square: $x"))
      .then(toString)
      .tap(x => println(s"After toString: $x"))
      .then(uppercase)
      .tap(x => println(s"After uppercase: $x"))
      .get
    
    println(s"Final result: $result")
    
    // More complex example with curried functions
    println("\nComplex pipeline:")
    
    def multiply(factor: Int)(x: Int): Int = x * factor
    def add(amount: Int)(x: Int): Int = x + amount
    def powerOf(exponent: Int)(x: Int): Int = Math.pow(x, exponent).toInt
    
    val result2 = Pipeline(2)
      .then(multiply(3))   // 6
      .then(add(4))        // 10
      .then(powerOf(2))    // 100
      .tap(x => println(s"Result: $x"))
      .get
  }
}
```

**Explanation:**
- `Pipeline` case class wraps values
- `then` applies transformation and returns new Pipeline
- `tap` performs side effect without changing value
- Type parameter `[A]` makes it work with any type
- `then[B]` allows type transformation (Int → String)
- Demonstrates function composition
- Pattern similar to Spark's DataFrame transformations

**Real-World Pattern (Spark-like):**
```scala
// Similar to Spark's transformation chain
case class DataPipeline(data: List[Int]) {
  def filter(predicate: Int => Boolean): DataPipeline = {
    DataPipeline(data.filter(predicate))
  }
  
  def map(transform: Int => Int): DataPipeline = {
    DataPipeline(data.map(transform))
  }
  
  def collect: List[Int] = data
}

val result = DataPipeline(List(1, 2, 3, 4, 5))
  .filter(_ % 2 == 0)   // Keep evens
  .map(_ * 2)           // Double
  .map(_ + 10)          // Add 10
  .collect
// Result: List(14, 18)
```

</details>

---

## Testing Your Programs

### Run and Verify:

1. **IntelliJ IDEA:**
   - Create Scala objects with code
   - Run each exercise
   - Verify output matches expected

2. **Experiment:**
   - Modify operations in higher-order functions
   - Test closure state independence
   - Measure performance with/without lazy evaluation
   - Create additional specialized functions via partial application

---

## Self-Assessment Checklist

After completing these exercises, you should be able to:
- [ ] Create higher-order functions that accept functions as parameters
- [ ] Return functions from functions
- [ ] Understand function type signatures like `(Int, Int) => Int`
- [ ] Create closures that capture variables from outer scope
- [ ] Use closures to encapsulate mutable state
- [ ] Define by-name parameters with `=>` syntax
- [ ] Understand when code is evaluated (eager vs lazy)
- [ ] Apply partial application to create specialized functions
- [ ] Write curried functions with multiple parameter groups
- [ ] Use underscore for partial application
- [ ] Compose functions to build complex behavior

---

## Common Mistakes to Avoid

1. **Modifying external state in functions:**
```scala
   var total = 0
   def bad(x: Int) = { total += x; total }  // Side effect
   def good(x: Int, acc: Int) = acc + x     // Pure
```

2. **Forgetting underscore in partial application:**
```scala
   def curry(a: Int)(b: Int) = a + b
   val partial = curry(5)     // Error!
   val partial = curry(5) _   // Correct
```

3. **Misunderstanding by-name evaluation:**
```scala
   def twice(x: => Int) = x + x
   var count = 0
   def inc() = { count += 1; count }
   twice(inc())  // Evaluates inc() TWICE (returns 3, not 2)
```

---

## Key Concepts Review

**Remember:**
- Higher-order functions accept/return functions
- Closures capture variables from outer scope
- By-name parameters delay evaluation
- Partial application fixes some arguments
- Currying enables sequential argument application
- These patterns enable powerful abstractions

---