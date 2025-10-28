
# Day 02: Practice Assignment

## Overview
This assignment helps reinforce your understanding of basic Scala syntax. Each exercise builds on essential concepts such as program structure, console printing, escape sequences, and imports.

---

## Exercise 1: Your First Scala Program

**Task:**  
Write a Scala program that prints your name and age on separate lines.

**Expected Output:**
```
My name is: [Your Name]
My age is: [Your Age]
```

**Requirements:**
- Use an `object` with a `main` method  
- Use two separate `println` statements  
- Replace `[Your Name]` and `[Your Age]` with your own details  

<details>
<summary>Show solution</summary>

```
object PersonalInfo {
  def main(args: Array[String]): Unit = {
    println("My name is: John Doe")
    println("My age is: 25")
  }
}
```

**Explanation:**  
The `object` defines the program structure.  
`def main(args: Array[String]): Unit` is the main entry point.  
Each `println` prints a line independently.
</details>

---

## Exercise 2: Working with Escape Sequences

**Task:**  
Create a program that displays a formatted address using escape sequences for proper spacing and line breaks.

**Expected Output:**
```
Name:    Jane Smith
Street:  123 Main Street
City:    "Springfield"
```

**Requirements:**
- Use `\t` for tab spacing  
- Use `\n` for new lines  
- Use `\"` to include quotes around the city name  
- Use a single `println` statement  

<details>
<summary>Show solution</summary>

```
object AddressFormatter {
  def main(args: Array[String]): Unit = {
    val address = "Name:\tJane Smith\nStreet:\t123 Main Street\nCity:\t\"Springfield\""
    println(address)
  }
}
```

**Explanation:**  
`\t` adds horizontal tab spaces.  
`\n` adds a line break.  
`\"` prints a double quote character.

**Alternative Solution:**
```
object AddressFormatter {
  def main(args: Array[String]): Unit = {
    println("Name:\tJane Smith")
    println("Street:\t123 Main Street")
    println("City:\t\"Springfield\"")
  }
}
```
</details>

---

## Exercise 3: Package and Import Practice

**Task:**  
Create a program that generates a random number between 0 and 99.

**Expected Output:**
```
Generated random number: [some number between 0-99]
```

**Requirements:**
- Add a package declaration: `package com.practice.day02`  
- Import `scala.util.Random`  
- Use `Random.nextInt(100)` to get a random number  
- Print the result  

<details>
<summary>Show solution</summary>

```
package com.practice.day02

import scala.util.Random

object RandomGenerator {
  def main(args: Array[String]): Unit = {
    val randomNumber = Random.nextInt(100)
    println("Generated random number: " + randomNumber)
  }
}
```

**Explanation:**  
`package` organizes your classes and objects.  
`import scala.util.Random` makes the `Random` class available.  
`Random.nextInt(100)` returns a random integer from 0 to 99.
</details>

---

## Bonus Challenge (Optional)

**Task:**  
Design a simple store receipt using escape sequences for neat alignment.

**Expected Output:**
```
========== RECEIPT ==========
Item            Price
----------------------------
Coffee          $3.50
Sandwich        $7.25
----------------------------
Total:          $10.75
=============================
```

<details>
<summary>Show solution</summary>

```
object Receipt {
  def main(args: Array[String]): Unit = {
    println("========== RECEIPT ==========")
    println("Item\t\tPrice")
    println("----------------------------")
    println("Coffee\t\t$3.50")
    println("Sandwich\t$7.25")
    println("----------------------------")
    println("Total:\t\t$10.75")
    println("=============================")
  }
}
```

**Explanation:**  
Tabs (`\t`) align columns for readability.  
Each `println` automatically adds a line break.

**Alternative Version:**
```
object Receipt {
  def main(args: Array[String]): Unit = {
    val receipt =
      "========== RECEIPT ==========\n" +
      "Item\t\tPrice\n" +
      "----------------------------\n" +
      "Coffee\t\t$3.50\n" +
      "Sandwich\t$7.25\n" +
      "----------------------------\n" +
      "Total:\t\t$10.75\n" +
      "============================="
    println(receipt)
  }
}
```
</details>

---

## Running Your Code

### Using Scala REPL
Enter the Scala REPL:
```
scala
```
Paste your code and press **Enter** to run.

### Using IntelliJ IDEA
1. Create a new Scala object.  
2. Paste your code.  
3. Right-click → **Run**.

### Using Command Line
```
scalac YourFileName.scala  # Compile
scala YourObjectName       # Run
```

---

## Self-Assessment Checklist

- [ ] Wrote a basic Scala program using `object` and `main` method  
- [ ] Printed output using `println`  
- [ ] Used escape sequences (`\n`, `\t`, `\"`) correctly  
- [ ] Added a package declaration  
- [ ] Imported external classes (for example, `Random`)  
- [ ] Understood program order: package → imports → object → main  


Once you have completed this practice set, move on to **Day 03** to learn about variables, data types, and type inference.

---