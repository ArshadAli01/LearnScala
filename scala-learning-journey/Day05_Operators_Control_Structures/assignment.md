# Day 05: Practice Assignment

## Instructions
Complete the following exercises to reinforce your understanding of operators, control structures, and pattern matching. Try solving them independently before referring to the solutions.

---

## Exercise 1: Temperature Converter with Conditions

**Task:**  
Create a program that converts Celsius to Fahrenheit and provides weather advice based on the temperature.

**Requirements:**
- Accept a Celsius temperature value
- Convert to Fahrenheit using: `F = (C × 9/5) + 32`
- Use `if-else` expressions to provide weather advice:
  - Below 0°C: "Freezing! Stay indoors"
  - 0-15°C: "Cold. Wear warm clothes"
  - 16-25°C: "Pleasant weather"
  - Above 25°C: "Hot! Stay hydrated"
- Display both temperatures and the advice

**Expected Output:**
```
Temperature: 22.0°C = 71.6°F
Weather Advice: Pleasant weather
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object TemperatureConverter {
  def main(args: Array[String]): Unit = {
    val celsius = 22.0
    
    // Convert to Fahrenheit
    val fahrenheit = (celsius * 9.0 / 5.0) + 32
    
    // Determine weather advice using if-else expression
    val advice = if (celsius < 0) {
      "Freezing! Stay indoors"
    } else if (celsius <= 15) {
      "Cold. Wear warm clothes"
    } else if (celsius <= 25) {
      "Pleasant weather"
    } else {
      "Hot! Stay hydrated"
    }
    
    // Display results
    println(f"Temperature: $celsius%.1f°C = $fahrenheit%.1f°F")
    println(s"Weather Advice: $advice")
  }
}
```

**Explanation:**
- Formula uses `.0` to ensure floating-point division
- `if-else` is an expression, so result assigned to `advice`
- Each condition checked sequentially from coldest to hottest
- f-interpolator formats temperatures to 1 decimal place
- This demonstrates `if-else` as value-producing expression

**Test with different values:**
```scala
val celsius = -5.0  // "Freezing! Stay indoors"
val celsius = 10.0  // "Cold. Wear warm clothes"
val celsius = 30.0  // "Hot! Stay hydrated"
```

</details>

---

## Exercise 2: Pattern Matching Day Planner

**Task:**  
Create a program that suggests activities based on the day of the week using pattern matching.

**Requirements:**
- Use pattern matching on day names (String)
- Provide different activities for:
  - Monday: Work-related
  - Friday: End of week
  - Saturday and Sunday: Weekend activities
  - Other weekdays: Regular work days
- Use the wildcard pattern for default case
- Print the day and suggested activity

**Expected Output:**
```
Monday: Start the week strong - Team meeting at 10 AM
Tuesday: Regular workday - Focus on project deliverables
Friday: TGIF! - Wrap up week and plan for Monday
Saturday: Weekend! - Time for hobbies and relaxation
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object DayPlanner {
  def main(args: Array[String]): Unit = {
    def planActivity(day: String): String = day match {
      case "Monday" => 
        "Start the week strong - Team meeting at 10 AM"
      case "Friday" => 
        "TGIF! - Wrap up week and plan for Monday"
      case "Saturday" | "Sunday" => 
        "Weekend! - Time for hobbies and relaxation"
      case "Tuesday" | "Wednesday" | "Thursday" => 
        "Regular workday - Focus on project deliverables"
      case _ => 
        "Invalid day"
    }
    
    val days = List("Monday", "Tuesday", "Friday", "Saturday", "Wednesday")
    
    for (day <- days) {
      val activity = planActivity(day)
      println(s"$day: $activity")
    }
  }
}
```

**Explanation:**
- `match` expression returns String value
- `|` operator combines multiple patterns (OR logic)
- Patterns evaluated in order - first match wins
- `_` wildcard catches any unmatched input
- Function `planActivity` demonstrates pattern matching as expression
- Loop iterates through test cases

**Enhanced Version with Pattern Guards:**
```scala
def planActivity(day: String, isHoliday: Boolean): String = day match {
  case d if isHoliday => s"$d is a holiday! - Enjoy your day off"
  case "Monday" => "Start the week strong"
  case "Friday" => "TGIF!"
  case "Saturday" | "Sunday" => "Weekend!"
  case _ => "Regular workday"
}
```

</details>

---

## Exercise 3: Number Classifier with Pattern Matching

**Task:**  
Write a program that classifies numbers using pattern matching with guards.

**Requirements:**
- Create a function that takes an `Int` parameter
- Use pattern matching with guards to classify:
  - Negative even numbers
  - Negative odd numbers
  - Zero
  - Positive even numbers
  - Positive odd numbers
- Test with a list of various numbers
- Print each number with its classification

**Expected Output:**
```
-4: Negative even number
-3: Negative odd number
0: Zero
2: Positive even number
5: Positive odd number
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object NumberClassifier {
  def main(args: Array[String]): Unit = {
    def classifyNumber(n: Int): String = n match {
      case x if x < 0 && x % 2 == 0 => "Negative even number"
      case x if x < 0 && x % 2 != 0 => "Negative odd number"
      case 0 => "Zero"
      case x if x > 0 && x % 2 == 0 => "Positive even number"
      case x if x > 0 && x % 2 != 0 => "Positive odd number"
      case _ => "Unknown"  // This case is unreachable but good practice
    }
    
    val numbers = List(-4, -3, 0, 2, 5, 10, -7, 15)
    
    for (num <- numbers) {
      val classification = classifyNumber(num)
      println(s"$num: $classification")
    }
  }
}
```

**Explanation:**
- Guards use `if` with conditions after pattern
- `x % 2 == 0` checks if number is even (divisible by 2)
- Multiple conditions combined with `&&` (logical AND)
- Variable `x` binds the matched value for use in guard
- Pattern matching with guards replaces complex nested if-else
- All cases are mutually exclusive (no overlap)

**Simplified Alternative:**
```scala
def classifyNumber(n: Int): String = n match {
  case 0 => "Zero"
  case x if x % 2 == 0 => s"${if (x < 0) "Negative" else "Positive"} even number"
  case x => s"${if (x < 0) "Negative" else "Positive"} odd number"
}
```

</details>

---

## Exercise 4: Collection Transformation with for-yield

**Task:**  
Process a list of numbers to create different transformations using `for-yield` expressions.

**Requirements:**
- Start with a list of numbers from 1 to 20
- Create transformation 1: Double all even numbers
- Create transformation 2: Square all numbers greater than 10
- Create transformation 3: Numbers divisible by 3, incremented by 1
- Print original list and all transformations

**Expected Output:**
```
Original: List(1, 2, 3, 4, 5, ... 20)
Even doubled: List(4, 8, 12, 16, 20, 24, 28, 32, 36, 40)
Squares > 10: List(121, 144, 169, 196, 225, 256, 289, 324, 361, 400)
Div by 3 + 1: List(4, 7, 10, 13, 16, 19, 22)
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object CollectionTransformation {
  def main(args: Array[String]): Unit = {
    val numbers = (1 to 20).toList
    
    // Transformation 1: Double all even numbers
    val evenDoubled = for {
      n <- numbers
      if n % 2 == 0
    } yield n * 2
    
    // Transformation 2: Square all numbers greater than 10
    val squaresAbove10 = for {
      n <- numbers
      if n > 10
    } yield n * n
    
    // Transformation 3: Numbers divisible by 3, incremented by 1
    val divBy3Plus1 = for {
      n <- numbers
      if n % 3 == 0
    } yield n + 1
    
    // Display results
    println(s"Original: $numbers")
    println(s"Even doubled: $evenDoubled")
    println(s"Squares > 10: $squaresAbove10")
    println(s"Div by 3 + 1: $divBy3Plus1")
  }
}
```

**Explanation:**
- `for-yield` creates new collection by transforming original
- Guard (`if` condition) filters which elements to process
- `yield` specifies transformation to apply
- Original collection remains unchanged (immutable)
- This pattern common in Spark transformations

**Equivalent functional methods:**
```scala
val evenDoubled = numbers.filter(_ % 2 == 0).map(_ * 2)
val squaresAbove10 = numbers.filter(_ > 10).map(n => n * n)
val divBy3Plus1 = numbers.filter(_ % 3 == 0).map(_ + 1)
```

**Multiple generators:**
```scala
// Combinations of two ranges
val pairs = for {
  i <- 1 to 3
  j <- 1 to 2
} yield (i, j)
// Result: List((1,1), (1,2), (2,1), (2,2), (3,1), (3,2))
```

</details>

---

## Bonus Challenge (Optional)

**Task:**  
Create a simple calculator that uses pattern matching to process commands and perform operations.

**Requirements:**
- Define operations as strings: "add", "subtract", "multiply", "divide"
- Accept two numbers and an operation
- Use pattern matching to execute the correct operation
- Handle division by zero with a guard
- Return result as String with appropriate message
- Test with multiple operations

**Expected Output:**
```
add 10 5: Result = 15.0
subtract 10 5: Result = 5.0
multiply 10 5: Result = 50.0
divide 10 5: Result = 2.0
divide 10 0: Error: Division by zero
power 10 2: Unknown operation
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object CalculatorChallenge {
  def main(args: Array[String]): Unit = {
    def calculate(operation: String, a: Double, b: Double): String = operation match {
      case "add" => 
        s"Result = ${a + b}"
      case "subtract" => 
        s"Result = ${a - b}"
      case "multiply" => 
        s"Result = ${a * b}"
      case "divide" if b != 0 => 
        s"Result = ${a / b}"
      case "divide" => 
        "Error: Division by zero"
      case _ => 
        "Unknown operation"
    }
    
    // Test cases
    val operations = List(
      ("add", 10.0, 5.0),
      ("subtract", 10.0, 5.0),
      ("multiply", 10.0, 5.0),
      ("divide", 10.0, 5.0),
      ("divide", 10.0, 0.0),
      ("power", 10.0, 2.0)
    )
    
    for ((op, a, b) <- operations) {
      val result = calculate(op, a, b)
      println(s"$op $a $b: $result")
    }
  }
}
```

**Explanation:**
- Pattern matching on operation string
- Guard `if b != 0` prevents division by zero
- Multiple `divide` cases: first with guard, second without (catches zero)
- Wildcard `_` catches unknown operations
- Tuple destructuring in for loop: `(op, a, b)`
- String interpolation in match results

**Enhanced with type matching:**
```scala
def calculate(command: Any): String = command match {
  case ("add", a: Double, b: Double) => s"${a + b}"
  case ("subtract", a: Double, b: Double) => s"${a - b}"
  case ("multiply", a: Double, b: Double) => s"${a * b}"
  case ("divide", a: Double, b: Double) if b != 0 => s"${a / b}"
  case ("divide", _, 0.0) => "Error: Division by zero"
  case _ => "Invalid command"
}

val result1 = calculate(("add", 10.0, 5.0))
val result2 = calculate(("divide", 10.0, 0.0))
```

</details>

---

## Testing Your Programs

### Run and Verify:

1. **IntelliJ IDEA:**
   - Create Scala object with code
   - Right-click → Run
   - Check console output matches expected

2. **Scala REPL:**
```bash
   scala
   :paste
   // Paste code
   // Ctrl+D
```

3. **Experiment:**
   - Change temperature values
   - Add new days to planner
   - Test edge cases (zero, negative numbers)

---

## Self-Assessment Checklist

After completing these exercises, you should be able to:
- [ ] Use arithmetic operators correctly (including integer division)
- [ ] Apply relational and logical operators
- [ ] Write `if-else` as expressions that return values
- [ ] Create multi-branch conditional logic
- [ ] Use pattern matching with value patterns
- [ ] Apply guards in pattern matching
- [ ] Handle multiple patterns with `|` operator
- [ ] Use wildcard pattern `_` for default cases
- [ ] Write `for` loops with guards
- [ ] Create new collections with `for-yield`
- [ ] Understand operator precedence
- [ ] Choose appropriate control structures for different scenarios

---

## Common Mistakes to Avoid

1. **Integer division:**
```scala
   val wrong = 10 / 3       // 3 (truncated)
   val correct = 10.0 / 3   // 3.333...
```

2. **Forgetting yield:**
```scala
   // Just prints, doesn't collect
   for (n <- 1 to 5) println(n)
   
   // Creates new list
   val list = for (n <- 1 to 5) yield n
```

3. **Non-exhaustive pattern matching:**
```scala
   // Compiler warns if case is missing
   val x = 5
   x match {
     case 1 => "one"
     // Missing cases - will throw MatchError at runtime
   }
```

4. **Guard condition placement:**
```scala
   // Wrong - if outside match
   // if (condition) value match { ... }
   
   // Correct - if in case
   value match {
     case x if condition => "matched"
   }
```

---

## Key Concepts Review

**Remember:**
- `if-else` returns value (is expression)
- Pattern matching more powerful than `switch`
- Guards add conditions to patterns with `if`
- `for-yield` creates new collections
- Operator precedence: multiply/divide before add/subtract
- Use parentheses for clarity
- Pattern matching avoids nested if-else

---

**Next Steps:** Once you've completed these exercises, you're ready for Day 06 where we'll learn about functions and methods!


---