# Day 08: Practice Assignment

## Instructions
Complete the following exercises to reinforce your understanding of classes, objects, constructors, methods, and access modifiers. Try solving them independently before referring to the solutions.

---

## Exercise 1: Basic Class Creation

**Task:**  
Create a `Book` class to manage book information with methods for displaying and updating data.

**Requirements:**
- Fields: title (immutable), author (immutable), year (immutable), price (mutable)
- Method `displayInfo()` to show all book information
- Method `applyDiscount(percentage: Double)` to reduce price
- Method `isClassic()` to return true if published before 1950
- Create multiple book objects and test all methods

**Expected Output:**
```
Book: The Great Gatsby by F. Scott Fitzgerald (1925) - $12.99
Is classic? true
After 20% discount: $10.39

Book: Clean Code by Robert Martin (2008) - $45.00
Is classic? false
After 10% discount: $40.50
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object BookExample {
  def main(args: Array[String]): Unit = {
    class Book(val title: String, val author: String, val year: Int, var price: Double) {
      def displayInfo(): String = {
        f"Book: $title by $author ($year) - $$${price}%.2f"
      }
      
      def applyDiscount(percentage: Double): Unit = {
        if (percentage > 0 && percentage <= 100) {
          price = price * (1 - percentage / 100)
          println(f"After ${percentage}%.0f%% discount: $$${price}%.2f")
        } else {
          println("Invalid discount percentage")
        }
      }
      
      def isClassic(): Boolean = {
        year < 1950
      }
    }
    
    // Create and test books
    val book1 = new Book("The Great Gatsby", "F. Scott Fitzgerald", 1925, 12.99)
    println(book1.displayInfo())
    println(s"Is classic? ${book1.isClassic()}")
    book1.applyDiscount(20)
    println()
    
    val book2 = new Book("Clean Code", "Robert Martin", 2008, 45.00)
    println(book2.displayInfo())
    println(s"Is classic? ${book2.isClassic()}")
    book2.applyDiscount(10)
  }
}
```

**Explanation:**
- `val` for title, author, year (immutable - shouldn't change)
- `var` for price (mutable - can apply discounts)
- `displayInfo()` uses string interpolation and f-interpolator for formatting
- `applyDiscount()` validates percentage and modifies price
- `isClassic()` returns Boolean based on year
- Each object maintains independent state

</details>

---

## Exercise 2: Bank Account with Encapsulation

**Task:**  
Create a `BankAccount` class that properly encapsulates balance with validation.

**Requirements:**
- Private balance field (cannot be accessed directly)
- Public methods: deposit, withdraw, getBalance, transfer
- Validation: deposits must be positive, withdrawals cannot exceed balance
- Track transaction count
- Method to display account summary

**Expected Output:**
```
Account: ACC001
Initial balance: $1000.00

Deposited $500.00
Current balance: $1500.00

Withdrew $300.00
Current balance: $1200.00

Insufficient funds for withdrawal of $2000.00
Current balance: $1200.00

Transferred $200.00 to ACC002
Sender balance: $1000.00
Receiver balance: $700.00

Account Summary:
Account Number: ACC001
Balance: $1000.00
Transactions: 3
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object BankAccountExample {
  def main(args: Array[String]): Unit = {
    class BankAccount(val accountNumber: String, initialBalance: Double) {
      private var balance: Double = initialBalance
      private var transactionCount: Int = 0
      
      def deposit(amount: Double): Boolean = {
        if (amount > 0) {
          balance += amount
          transactionCount += 1
          println(f"Deposited $$${amount}%.2f")
          true
        } else {
          println("Deposit amount must be positive")
          false
        }
      }
      
      def withdraw(amount: Double): Boolean = {
        if (amount > 0 && amount <= balance) {
          balance -= amount
          transactionCount += 1
          println(f"Withdrew $$${amount}%.2f")
          true
        } else {
          println(f"Insufficient funds for withdrawal of $$${amount}%.2f")
          false
        }
      }
      
      def getBalance(): Double = balance
      
      def transfer(amount: Double, toAccount: BankAccount): Boolean = {
        if (withdraw(amount)) {
          toAccount.deposit(amount)
          println(f"Transferred $$${amount}%.2f to ${toAccount.accountNumber}")
          true
        } else {
          false
        }
      }
      
      def displaySummary(): Unit = {
        println("\nAccount Summary:")
        println(s"Account Number: $accountNumber")
        println(f"Balance: $$${balance}%.2f")
        println(s"Transactions: $transactionCount")
      }
    }
    
    // Testing
    val account1 = new BankAccount("ACC001", 1000.00)
    val account2 = new BankAccount("ACC002", 500.00)
    
    println(s"Account: ${account1.accountNumber}")
    println(f"Initial balance: $$${account1.getBalance()}%.2f\n")
    
    account1.deposit(500)
    println(f"Current balance: $$${account1.getBalance()}%.2f\n")
    
    account1.withdraw(300)
    println(f"Current balance: $$${account1.getBalance()}%.2f\n")
    
    account1.withdraw(2000)
    println(f"Current balance: $$${account1.getBalance()}%.2f\n")
    
    account1.transfer(200, account2)
    println(f"Sender balance: $$${account1.getBalance()}%.2f")
    println(f"Receiver balance: $$${account2.getBalance()}%.2f")
    
    account1.displaySummary()
  }
}
```

**Explanation:**
- `balance` is private - cannot be accessed directly
- `transactionCount` is also private
- Public methods provide controlled access
- Validation in deposit and withdraw
- `transfer` uses existing methods for consistency
- Encapsulation protects invariants (balance never negative)

</details>

---

## Exercise 3: Multiple Constructors

**Task:**  
Create a `Student` class with primary and auxiliary constructors for flexible object creation.

**Requirements:**
- Primary constructor: name, id, age, gpa
- Auxiliary constructor 1: name, id, age (default GPA = 0.0)
- Auxiliary constructor 2: name, id (default age = 18, default GPA = 0.0)
- Method `updateGPA(newGPA: Double)` with validation (0.0-4.0)
- Method `isHonorStudent()` returns true if GPA >= 3.5
- Method `displayInfo()` shows all student information

**Expected Output:**
```
Student: Alice, ID: S001, Age: 20, GPA: 3.8
Honor student: true

Student: Bob, ID: S002, Age: 19, GPA: 0.0
Honor student: false
Updated GPA to 3.6
Honor student: true

Student: Charlie, ID: S003, Age: 18, GPA: 0.0
Honor student: false
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object StudentExample {
  def main(args: Array[String]): Unit = {
    class Student(val name: String, val id: String, val age: Int, private var gpa: Double) {
      // Validate GPA in primary constructor
      require(gpa >= 0.0 && gpa <= 4.0, "GPA must be between 0.0 and 4.0")
      
      // Auxiliary constructor 1: without GPA
      def this(name: String, id: String, age: Int) = {
        this(name, id, age, 0.0)
      }
      
      // Auxiliary constructor 2: without age and GPA
      def this(name: String, id: String) = {
        this(name, id, 18, 0.0)
      }
      
      def updateGPA(newGPA: Double): Boolean = {
        if (newGPA >= 0.0 && newGPA <= 4.0) {
          gpa = newGPA
          println(f"Updated GPA to $newGPA%.1f")
          true
        } else {
          println("GPA must be between 0.0 and 4.0")
          false
        }
      }
      
      def getGPA(): Double = gpa
      
      def isHonorStudent(): Boolean = gpa >= 3.5
      
      def displayInfo(): String = {
        f"Student: $name, ID: $id, Age: $age, GPA: $gpa%.1f"
      }
    }
    
    // Using different constructors
    val student1 = new Student("Alice", "S001", 20, 3.8)
    println(student1.displayInfo())
    println(s"Honor student: ${student1.isHonorStudent()}\n")
    
    val student2 = new Student("Bob", "S002", 19)
    println(student2.displayInfo())
    println(s"Honor student: ${student2.isHonorStudent()}")
    student2.updateGPA(3.6)
    println(s"Honor student: ${student2.isHonorStudent()}\n")
    
    val student3 = new Student("Charlie", "S003")
    println(student3.displayInfo())
    println(s"Honor student: ${student3.isHonorStudent()}")
  }
}
```

**Explanation:**
- Primary constructor with all parameters
- Auxiliary constructors call primary with defaults
- `require` validates GPA in constructor
- GPA is private but accessible via getter
- `updateGPA` provides controlled modification
- Each constructor provides different level of specification

</details>

---

## Exercise 4: Access Modifiers Practice

**Task:**  
Create a `SmartLight` class demonstrating proper use of access modifiers.

**Requirements:**
- Private fields: brightness (0-100), isOn, color
- Protected field: energyConsumption
- Public methods: turnOn, turnOff, setBrightness, setColor, getStatus
- Private method: calculateEnergyConsumption
- Brightness validation (0-100)
- Track total energy consumed

**Expected Output:**
```
Light Status: OFF, Brightness: 0%, Color: White, Energy: 0.0W

Light turned ON
Light Status: ON, Brightness: 50%, Color: White, Energy: 25.0W

Brightness set to 80%
Light Status: ON, Brightness: 80%, Color: White, Energy: 40.0W

Color changed to Blue
Light Status: ON, Brightness: 80%, Color: Blue, Energy: 40.0W

Light turned OFF
Light Status: OFF, Brightness: 80%, Color: Blue, Energy: 0.0W
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object SmartLightExample {
  def main(args: Array[String]): Unit = {
    class SmartLight {
      private var brightness: Int = 0
      private var isOn: Boolean = false
      private var color: String = "White"
      protected var energyConsumption: Double = 0.0
      
      def turnOn(): Unit = {
        isOn = true
        if (brightness == 0) brightness = 50  // Default brightness
        calculateEnergyConsumption()
        println("Light turned ON")
      }
      
      def turnOff(): Unit = {
        isOn = false
        calculateEnergyConsumption()
        println("Light turned OFF")
      }
      
      def setBrightness(level: Int): Boolean = {
        if (level >= 0 && level <= 100) {
          brightness = level
          calculateEnergyConsumption()
          println(s"Brightness set to $level%")
          true
        } else {
          println("Brightness must be between 0 and 100")
          false
        }
      }
      
      def setColor(newColor: String): Unit = {
        color = newColor
        println(s"Color changed to $newColor")
      }
      
      def getStatus(): String = {
        val status = if (isOn) "ON" else "OFF"
        f"Light Status: $status, Brightness: $brightness%%, Color: $color, Energy: $energyConsumption%.1fW"
      }
      
      private def calculateEnergyConsumption(): Unit = {
        energyConsumption = if (isOn) brightness * 0.5 else 0.0
      }
    }
    
    val light = new SmartLight()
    println(light.getStatus())
    println()
    
    light.turnOn()
    println(light.getStatus())
    println()
    
    light.setBrightness(80)
    println(light.getStatus())
    println()
    
    light.setColor("Blue")
    println(light.getStatus())
    println()
    
    light.turnOff()
    println(light.getStatus())
  }
}
```

**Explanation:**
- All state is private (brightness, isOn, color)
- Protected energyConsumption (could be accessed by subclasses)
- Public interface methods control behavior
- Private `calculateEnergyConsumption()` is internal logic
- Cannot directly access or modify private fields from outside
- Demonstrates encapsulation: hide implementation, expose interface

</details>

---

## Bonus Challenge (Optional)

**Task:**  
Create a `Library` class that manages a collection of books with borrowing functionality.

**Requirements:**
- Store books in a private collection
- Methods: addBook, removeBook, borrowBook, returnBook, listAvailableBooks
- Track which books are borrowed
- Prevent borrowing unavailable books
- Display library statistics

**Expected Output:**
```
Library: City Library

Added: The Great Gatsby
Added: 1984
Added: To Kill a Mockingbird

Available Books:
1. The Great Gatsby
2. 1984
3. To Kill a Mockingbird

Borrowed: 1984

Available Books:
1. The Great Gatsby
2. To Kill a Mockingbird

Returned: 1984

Library Statistics:
Total Books: 3
Available: 3
Borrowed: 0
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object LibraryChallenge {
  def main(args: Array[String]): Unit = {
    case class Book(title: String, author: String, var isAvailable: Boolean = true)
    
    class Library(val name: String) {
      private var books: List[Book] = List()
      
      def addBook(title: String, author: String): Unit = {
        books = books :+ Book(title, author)
        println(s"Added: $title")
      }
      
      def removeBook(title: String): Boolean = {
        val initialSize = books.size
        books = books.filterNot(_.title == title)
        if (books.size < initialSize) {
          println(s"Removed: $title")
          true
        } else {
          println(s"Book not found: $title")
          false
        }
      }
      
      def borrowBook(title: String): Boolean = {
        books.find(b => b.title == title && b.isAvailable) match {
          case Some(book) =>
            book.isAvailable = false
            println(s"Borrowed: $title")
            true
          case None =>
            println(s"Book not available: $title")
            false
        }
      }
      
      def returnBook(title: String): Boolean = {
        books.find(_.title == title) match {
          case Some(book) =>
            book.isAvailable = true
            println(s"Returned: $title")
            true
          case None =>
            println(s"Book not in library: $title")
            false
        }
      }
      
      def listAvailableBooks(): Unit = {
        val available = books.filter(_.isAvailable)
        println("\nAvailable Books:")
        available.zipWithIndex.foreach { case (book, index) =>
          println(s"${index + 1}. ${book.title}")
        }
      }
      
      def displayStatistics(): Unit = {
        val available = books.count(_.isAvailable)
        val borrowed = books.size - available
        println("\nLibrary Statistics:")
        println(s"Total Books: ${books.size}")
        println(s"Available: $available")
        println(s"Borrowed: $borrowed")
      }
    }
    
    val library = new Library("City Library")
    println(s"Library: ${library.name}\n")
    
    library.addBook("The Great Gatsby", "F. Scott Fitzgerald")
    library.addBook("1984", "George Orwell")
    library.addBook("To Kill a Mockingbird", "Harper Lee")
    
    library.listAvailableBooks()
    
    library.borrowBook("1984")
    library.listAvailableBooks()
    
    library.returnBook("1984")
    
    library.displayStatistics()
  }
}
```

**Explanation:**
- Case class `Book` for simple data structure
- Private `books` list encapsulates collection
- `addBook` appends to list
- `borrowBook` finds book and marks unavailable
- `returnBook` marks book as available
- Pattern matching for safe book lookup
- Demonstrates managing collections within classes

</details>

---

## Testing Your Programs

### Run and Verify:

1. **IntelliJ IDEA:**
   - Create Scala objects with code
   - Run each exercise
   - Verify output matches expected

2. **Experiment:**
   - Try invalid inputs (negative prices, invalid GPA)
   - Create multiple objects and verify independence
   - Test edge cases (empty strings, boundary values)

---

## Self-Assessment Checklist

After completing these exercises, you should be able to:
- [ ] Define classes with `class` keyword
- [ ] Distinguish between `val` (immutable) and `var` (mutable) fields
- [ ] Create objects with `new` keyword
- [ ] Define methods with `def` keyword
- [ ] Use primary constructor in class header
- [ ] Create auxiliary constructors with `def this(...)`
- [ ] Apply access modifiers: public (default), private, protected
- [ ] Encapsulate state with private fields
- [ ] Provide public interface methods
- [ ] Validate input in constructors and methods
- [ ] Understand when to use val vs var

---

## Common Mistakes to Avoid

1. **Forgetting val/var in constructor:**
```scala
   class Person(name: String)  // name not accessible
   class Person(val name: String)  // name is public field
```

2. **Not calling primary in auxiliary constructor:**
```scala
   def this(name: String) = {
     // this.name = name  // Error!
     this(name, 18)  // Must call primary
   }
```

3. **Accessing private fields from outside:**
```scala
   class Example {
     private var x = 10
   }
   val ex = new Example()
   // ex.x = 20  // Error - x is private
```

4. **Modifying val fields:**
```scala
   class Example(val x: Int)
   val ex = new Example(10)
   // ex.x = 20  // Error - val is immutable
```

---

## Key Concepts Review

**Remember:**
- Classes are blueprints, objects are instances
- `val` = immutable, `var` = mutable
- Constructors initialize objects
- Encapsulate with private fields
- Provide public methods for access
- Validate in constructors and methods

---