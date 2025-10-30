# Day 04: Practice Assignment

## Instructions
Complete the following exercises to reinforce your understanding of variables (`val` vs `var`), type inference, and best practices. Try solving them independently before referring to the solutions.

---

## Exercise 1: Immutability Practice

**Task:**  
Create a program that demonstrates the difference between `val` and `var` by managing a simple inventory system.

**Requirements:**
- Declare immutable variables for product name, original price, and tax rate
- Use a mutable variable to track quantity in stock
- Simulate selling items by decreasing the quantity
- Calculate and display the final price with tax
- Try to modify an immutable variable and observe the error (comment it out after testing)

**Expected Output:**
```
Product: Laptop
Original Price: $999.99
Tax Rate: 8.0%
Initial Stock: 50
After selling 5 units: 45 units remaining
Final Price (with tax): $1079.99
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object InventorySystem {
  def main(args: Array[String]): Unit = {
    // Immutable product details - should never change
    val productName = "Laptop"
    val originalPrice = 999.99
    val taxRate = 0.08  // 8%
    
    // Mutable inventory count - changes as items are sold
    var stockQuantity = 50
    
    println(s"Product: $productName")
    println(f"Original Price: $$${originalPrice}%.2f")
    println(f"Tax Rate: ${taxRate * 100}%.1f%%")
    println(s"Initial Stock: $stockQuantity")
    
    // Simulate selling items
    val unitsSold = 5
    stockQuantity -= unitsSold
    
    println(s"After selling $unitsSold units: $stockQuantity units remaining")
    
    // Calculate final price with tax
    val finalPrice = originalPrice * (1 + taxRate)
    println(f"Final Price (with tax): $$${finalPrice}%.2f")
    
    // Try to modify immutable variable (this will cause compilation error)
    // productName = "Desktop"  // Error: reassignment to val
    // originalPrice = 899.99   // Error: reassignment to val
  }
}
```

**Explanation:**
- `val` used for product details that shouldn't change (name, price, tax rate)
- `var` used for stock quantity which decreases as items are sold
- Attempting to reassign `val` variables causes compilation error
- `stockQuantity -= unitsSold` is shorthand for `stockQuantity = stockQuantity - unitsSold`
- Immutable `finalPrice` computed from immutable values
- This pattern ensures product details remain consistent throughout execution

</details>

---

## Exercise 2: Type Inference Exploration

**Task:**  
Write a program that declares variables using type inference and then explicitly prints their inferred types.

**Requirements:**
- Declare at least 6 variables of different types without explicit type annotations
- Include: integer, decimal, string, boolean, list, and map
- Print each variable's value and its inferred type
- Add a comment for each variable indicating what type you expect

**Expected Output:**
```
Variable: 42, Type: Integer
Variable: 3.14159, Type: Double
Variable: Hello Scala, Type: String
Variable: true, Type: Boolean
Variable: List(1, 2, 3, 4, 5), Type: List
Variable: Map(name -> Alice, age -> 25), Type: Map
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object TypeInferenceExploration {
  def main(args: Array[String]): Unit = {
    // Let compiler infer types
    val integerValue = 42                        // Expect: Int
    val decimalValue = 3.14159                   // Expect: Double
    val textValue = "Hello Scala"                // Expect: String
    val booleanValue = true                      // Expect: Boolean
    val listValue = List(1, 2, 3, 4, 5)          // Expect: List[Int]
    val mapValue = Map("name" -> "Alice", "age" -> 25)  // Expect: Map[String, Any]
    
    // Print values and types
    println(s"Variable: $integerValue, Type: ${integerValue.getClass.getSimpleName}")
    println(s"Variable: $decimalValue, Type: ${decimalValue.getClass.getSimpleName}")
    println(s"Variable: $textValue, Type: ${textValue.getClass.getSimpleName}")
    println(s"Variable: $booleanValue, Type: ${booleanValue.getClass.getSimpleName}")
    println(s"Variable: $listValue, Type: ${listValue.getClass.getSimpleName}")
    println(s"Variable: $mapValue, Type: ${mapValue.getClass.getSimpleName}")
    
    // Additional: Show type in a more detailed way
    println("\nDetailed type information:")
    println(s"listValue full type: ${listValue.getClass}")
    println(s"mapValue full type: ${mapValue.getClass}")
  }
}
```

**Explanation:**
- Compiler automatically infers types from right-hand side expressions
- Integer literals → `Int`
- Decimal literals → `Double`
- String literals → `String`
- Boolean literals → `Boolean`
- `List(1, 2, 3)` → `List[Int]` (list of integers)
- `Map("name" -> "Alice", "age" -> 25)` → `Map[String, Any]` (mixed value types)
- `getClass.getSimpleName` returns simplified type name
- `getClass` returns full qualified type name with package information

</details>

---

## Exercise 3: Configuration Management

**Task:**  
Create a realistic configuration system for an application using immutable values, simulating reading configuration and using it throughout the application.

**Requirements:**
- Store all configuration in a `val` Map
- Configuration should include: app name, version, max retries, timeout, and debug mode
- Extract individual configuration values into separate `val` variables
- Use a `var` to simulate a retry counter
- Implement a simple retry loop that respects max retries from configuration
- Print configuration details and retry attempts

**Expected Output:**
```
Application Configuration
========================
App Name: DataProcessor
Version: 1.0.0
Max Retries: 3
Timeout: 30 seconds
Debug Mode: true
========================
Attempt 1 of 3
Attempt 2 of 3
Processing successful!
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object ConfigurationManagement {
  def main(args: Array[String]): Unit = {
    // Immutable configuration - should never change during execution
    val config = Map(
      "app.name" -> "DataProcessor",
      "app.version" -> "1.0.0",
      "max.retries" -> 3,
      "timeout.seconds" -> 30,
      "debug.mode" -> true
    )
    
    // Extract configuration values
    val appName = config("app.name")
    val appVersion = config("app.version")
    val maxRetries = config("max.retries").asInstanceOf[Int]
    val timeout = config("timeout.seconds").asInstanceOf[Int]
    val debugMode = config("debug.mode").asInstanceOf[Boolean]
    
    // Display configuration
    println("Application Configuration")
    println("=" * 24)
    println(s"App Name: $appName")
    println(s"Version: $appVersion")
    println(s"Max Retries: $maxRetries")
    println(s"Timeout: $timeout seconds")
    println(s"Debug Mode: $debugMode")
    println("=" * 24)
    
    // Simulate retry logic with mutable counter
    var currentAttempt = 0
    var success = false
    
    while (currentAttempt < maxRetries && !success) {
      currentAttempt += 1
      println(s"Attempt $currentAttempt of $maxRetries")
      
      // Simulate random success (let's say succeeds on attempt 2)
      if (currentAttempt == 2) {
        success = true
        println("Processing successful!")
      }
    }
    
    if (!success) {
      println(s"Processing failed after $maxRetries attempts")
    }
  }
}
```

**Explanation:**
- All configuration stored in immutable `Map` - prevents accidental changes
- Individual values extracted for readability
- `asInstanceOf[Type]` casts `Any` values to specific types (needed because Map values are `Any`)
- `currentAttempt` is `var` because it increments (legitimate mutable use case)
- `success` flag is `var` to track processing status
- While loop respects `maxRetries` from configuration
- This pattern common in real applications: configuration as immutable data

**Better Type-Safe Alternative (Preview of future concepts):**
```scala
// Define configuration with proper types
case class AppConfig(
  appName: String,
  appVersion: String,
  maxRetries: Int,
  timeoutSeconds: Int,
  debugMode: Boolean
)

val config = AppConfig(
  appName = "DataProcessor",
  appVersion = "1.0.0",
  maxRetries = 3,
  timeoutSeconds = 30,
  debugMode = true
)

// Now config.maxRetries is already Int, no casting needed!
```

</details>

---

## Bonus Challenge (Optional)

**Task:**  
Implement a simple bank account system that demonstrates proper use of `val` and `var` with transaction tracking.

**Requirements:**
- Account holder name and account number should be immutable
- Balance should be mutable (it changes with transactions)
- Create methods to deposit and withdraw (using simple functions for now)
- Track number of transactions with a mutable counter
- Implement balance inquiry
- Ensure withdrawal doesn't allow negative balance

**Expected Output:**
```
Account: ABC123 - John Doe
Initial Balance: $1000.00
Deposited: $500.00
New Balance: $1500.00
Withdrawn: $300.00
New Balance: $1200.00
Insufficient funds for withdrawal of $2000.00
Final Balance: $1200.00
Total Transactions: 2
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object BankAccountSystem {
  def main(args: Array[String]): Unit = {
    // Immutable account details
    val accountNumber = "ABC123"
    val accountHolder = "John Doe"
    
    // Mutable balance and transaction count
    var balance = 1000.00
    var transactionCount = 0
    
    // Display initial state
    println(s"Account: $accountNumber - $accountHolder")
    println(f"Initial Balance: $$${balance}%.2f")
    println()
    
    // Deposit function simulation
    def deposit(amount: Double): Unit = {
      balance += amount
      transactionCount += 1
      println(f"Deposited: $$${amount}%.2f")
      println(f"New Balance: $$${balance}%.2f")
      println()
    }
    
    // Withdraw function simulation
    def withdraw(amount: Double): Unit = {
      if (balance >= amount) {
        balance -= amount
        transactionCount += 1
        println(f"Withdrawn: $$${amount}%.2f")
        println(f"New Balance: $$${balance}%.2f")
        println()
      } else {
        println(f"Insufficient funds for withdrawal of $$${amount}%.2f")
        println()
      }
    }
    
    // Perform transactions
    deposit(500.00)
    withdraw(300.00)
    withdraw(2000.00)  // This should fail
    
    // Final summary
    println(f"Final Balance: $$${balance}%.2f")
    println(s"Total Transactions: $transactionCount")
  }
}
```

**Explanation:**
- `accountNumber` and `accountHolder` are `val` - these never change
- `balance` is `var` - it changes with each transaction
- `transactionCount` is `var` - increments with successful operations
- Deposit adds to balance unconditionally
- Withdraw checks balance first to prevent negative amounts
- Functions defined inside `main` can access and modify variables in scope
- This demonstrates appropriate use of mutability for state that genuinely changes

**Note for Future Learning:**
In real Scala, we'd use classes and encapsulation:
```scala
class BankAccount(val accountNumber: String, val holder: String, private var balance: Double) {
  private var transactionCount = 0
  
  def deposit(amount: Double): Unit = {
    balance += amount
    transactionCount += 1
  }
  
  def withdraw(amount: Double): Boolean = {
    if (balance >= amount) {
      balance -= amount
      transactionCount += 1
      true
    } else {
      false
    }
  }
  
  def getBalance: Double = balance
  def getTransactionCount: Int = transactionCount
}
```

</details>

---

## Testing Your Programs

### Compilation and Execution:

1. **IntelliJ IDEA:**
   - Copy code into new Scala object
   - Click green play button
   - Observe output in console

2. **Command Line:**
```bash
   scalac InventorySystem.scala
   scala InventorySystem
```

3. **Scala REPL (for quick testing):**
```bash
   scala
   :paste
   // Paste your code
   // Ctrl+D to execute
```

---

## Self-Assessment Checklist

After completing these exercises, you should be able to:
- [ ] Explain the difference between `val` and `var`
- [ ] Choose appropriate declaration keyword based on use case
- [ ] Understand why immutability is preferred
- [ ] Let the compiler infer types for simple cases
- [ ] Recognize when explicit type annotations are needed
- [ ] Use proper naming conventions (camelCase for variables)
- [ ] Implement simple retry logic with mutable counters
- [ ] Store configuration as immutable data structures
- [ ] Understand type inference for collections (List, Map)
- [ ] Use compound assignment operators (+=, -=)

---

## Common Mistakes to Watch For

1. **Overusing `var`:**
```scala
   // Bad - unnecessary mutability
   var result = 0
   result = calculation()
   
   // Good - immutable
   val result = calculation()
```

2. **Trying to modify `val`:**
```scala
   val name = "Alice"
   // name = "Bob"  // Compilation error!
```

3. **Not using type inference:**
```scala
   // Overly verbose
   val number: Int = 42
   val text: String = "Hello"
   
   // Simpler (compiler infers)
   val number = 42
   val text = "Hello"
```

4. **Poor naming:**
```scala
   val x = 100  // What is x?
   val temp = "data"  // Vague
   
   // Better
   val maxRetries = 100
   val customerName = "data"
```

---

## Key Concepts Review

**Remember:**
- `val` = immutable (can't reassign)
- `var` = mutable (can reassign)
- Prefer `val` by default
- Use `var` only when needed (counters, loops, performance)
- Compiler infers types - use explicit types for clarity when needed
- Immutability = safety, thread-safety, optimization
- Follow naming conventions: camelCase for variables

---
