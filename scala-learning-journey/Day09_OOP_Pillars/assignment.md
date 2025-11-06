# Day 09: Practice Assignment

## Instructions
Complete the following exercises to reinforce your understanding of inheritance, polymorphism, abstract classes, and encapsulation. Try solving them independently before referring to the solutions.

---

## Exercise 1: Basic Inheritance and Method Overriding

**Task:**  
Create an employee management system using inheritance.

**Requirements:**
- Create base class `Employee` with: name, id, baseSalary
- Method `calculateSalary()` returns baseSalary
- Method `getDetails()` returns employee information
- Create `Manager` subclass extending Employee
- Manager adds: teamSize, bonus
- Override `calculateSalary()` to include bonus
- Override `getDetails()` to include team size
- Create `Developer` subclass with: programmingLanguage, projectCount
- Override `calculateSalary()` with project bonus (projectCount * 1000)
- Test with multiple employees

**Expected Output:**
```
Employee: Alice, ID: E001
Salary: $50000.00

Manager: Bob, ID: M001, Team Size: 5
Salary: $65000.00

Developer: Charlie, ID: D001, Language: Scala, Projects: 3
Salary: $58000.00
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object EmployeeHierarchy {
  def main(args: Array[String]): Unit = {
    class Employee(val name: String, val id: String, val baseSalary: Double) {
      def calculateSalary(): Double = baseSalary
      
      def getDetails(): String = {
        s"Employee: $name, ID: $id"
      }
    }
    
    class Manager(name: String, id: String, baseSalary: Double, 
                  val teamSize: Int, val bonus: Double) 
      extends Employee(name, id, baseSalary) {
      
      override def calculateSalary(): Double = baseSalary + bonus
      
      override def getDetails(): String = {
        s"Manager: $name, ID: $id, Team Size: $teamSize"
      }
    }
    
    class Developer(name: String, id: String, baseSalary: Double,
                    val programmingLanguage: String, val projectCount: Int)
      extends Employee(name, id, baseSalary) {
      
      override def calculateSalary(): Double = baseSalary + (projectCount * 1000)
      
      override def getDetails(): String = {
        s"Developer: $name, ID: $id, Language: $programmingLanguage, Projects: $projectCount"
      }
    }
    
    val employee = new Employee("Alice", "E001", 50000)
    val manager = new Manager("Bob", "M001", 50000, 5, 15000)
    val developer = new Developer("Charlie", "D001", 55000, "Scala", 3)
    
    val employees = List(employee, manager, developer)
    
    employees.foreach { emp =>
      println(emp.getDetails())
      println(f"Salary: $$${emp.calculateSalary()}%.2f")
      println()
    }
  }
}
```

**Explanation:**
- `Employee` is base class with common fields and methods
- `Manager` and `Developer` extend Employee
- Both override `calculateSalary()` with their own logic
- Both override `getDetails()` to include additional information
- Polymorphism: list of Employee can hold all types
- Each type uses its own overridden methods

</details>

---

## Exercise 2: Abstract Classes and Polymorphism

**Task:**  
Create a shape calculation system using abstract classes.

**Requirements:**
- Abstract class `Shape` with abstract methods: `area()`, `perimeter()`
- Concrete method `displayInfo()` that uses area and perimeter
- Create `Circle` subclass with radius
- Create `Rectangle` subclass with width and height
- Create `Triangle` subclass with three sides
- Function `compareShapes()` that compares areas of two shapes
- Test with list of different shapes

**Expected Output:**
```
Circle (radius: 5.0)
Area: 78.54
Perimeter: 31.42
---
Rectangle (4.0 x 6.0)
Area: 24.00
Perimeter: 20.00
---
Triangle (sides: 3.0, 4.0, 5.0)
Area: 6.00
Perimeter: 12.00
---
Circle has larger area than Rectangle
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object ShapeHierarchy {
  def main(args: Array[String]): Unit = {
    abstract class Shape {
      def area(): Double
      def perimeter(): Double
      def shapeName(): String
      
      def displayInfo(): Unit = {
        println(shapeName())
        println(f"Area: ${area()}%.2f")
        println(f"Perimeter: ${perimeter()}%.2f")
        println("---")
      }
    }
    
    class Circle(val radius: Double) extends Shape {
      override def area(): Double = Math.PI * radius * radius
      override def perimeter(): Double = 2 * Math.PI * radius
      override def shapeName(): String = s"Circle (radius: $radius)"
    }
    
    class Rectangle(val width: Double, val height: Double) extends Shape {
      override def area(): Double = width * height
      override def perimeter(): Double = 2 * (width + height)
      override def shapeName(): String = s"Rectangle ($width x $height)"
    }
    
    class Triangle(val side1: Double, val side2: Double, val side3: Double) extends Shape {
      override def area(): Double = {
        val s = perimeter() / 2
        Math.sqrt(s * (s - side1) * (s - side2) * (s - side3))
      }
      override def perimeter(): Double = side1 + side2 + side3
      override def shapeName(): String = s"Triangle (sides: $side1, $side2, $side3)"
    }
    
    def compareShapes(shape1: Shape, shape2: Shape): Unit = {
      val name1 = shape1.shapeName().split("\\(")(0).trim
      val name2 = shape2.shapeName().split("\\(")(0).trim
      
      if (shape1.area() > shape2.area()) {
        println(s"$name1 has larger area than $name2")
      } else if (shape1.area() < shape2.area()) {
        println(s"$name2 has larger area than $name1")
      } else {
        println(s"$name1 and $name2 have equal areas")
      }
    }
    
    val shapes: List[Shape] = List(
      new Circle(5.0),
      new Rectangle(4.0, 6.0),
      new Triangle(3.0, 4.0, 5.0)
    )
    
    shapes.foreach(_.displayInfo())
    
    compareShapes(shapes(0), shapes(1))
  }
}
```

**Explanation:**
- `Shape` is abstract with abstract methods
- Cannot instantiate `Shape` directly
- All concrete subclasses must implement abstract methods
- `displayInfo()` is concrete, uses abstract methods
- Polymorphism allows `List[Shape]` to hold all types
- `compareShapes` works with any Shape subclass

</details>

---

## Exercise 3: Encapsulation with Validation

**Task:**  
Create a `TemperatureControl` class with proper encapsulation.

**Requirements:**
- Private fields: currentTemp, targetTemp, minTemp, maxTemp
- Constructor validates temperature ranges
- Public getter for currentTemp (read-only)
- Public setter for targetTemp with validation
- Method `adjustTemperature()` moves current toward target
- Method `getStatus()` returns formatted status
- Private helper method `isValidTemperature()`
- Test boundary conditions

**Expected Output:**
```
Status: Current: 20.0°C, Target: 24.0°C, Range: 10.0°C to 30.0°C

Adjusting temperature...
Status: Current: 22.0°C, Target: 24.0°C, Range: 10.0°C to 30.0°C

Adjusting temperature...
Status: Current: 24.0°C, Target: 24.0°C, Range: 10.0°C to 30.0°C

Target reached!

Cannot set target to 35.0°C (out of range)
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object TemperatureControl {
  def main(args: Array[String]): Unit = {
    class TemperatureController(
      initialTemp: Double,
      initialTarget: Double,
      val minTemp: Double = 10.0,
      val maxTemp: Double = 30.0
    ) {
      private var currentTemp: Double = initialTemp
      private var targetTemp: Double = initialTarget
      
      require(isValidTemperature(initialTemp), "Initial temperature out of range")
      require(isValidTemperature(initialTarget), "Target temperature out of range")
      
      // Getter for current temperature (read-only)
      def getCurrentTemp(): Double = currentTemp
      
      // Setter for target with validation
      def setTarget(temp: Double): Boolean = {
        if (isValidTemperature(temp)) {
          targetTemp = temp
          println(f"Target set to $temp%.1f°C")
          true
        } else {
          println(f"Cannot set target to $temp%.1f°C (out of range)")
          false
        }
      }
      
      def adjustTemperature(): Unit = {
        if (currentTemp < targetTemp) {
          currentTemp = Math.min(currentTemp + 2, targetTemp)
          println("Adjusting temperature...")
        } else if (currentTemp > targetTemp) {
          currentTemp = Math.max(currentTemp - 2, targetTemp)
          println("Adjusting temperature...")
        } else {
          println("Target reached!")
        }
      }
      
      def getStatus(): String = {
        f"Status: Current: $currentTemp%.1f°C, Target: $targetTemp%.1f°C, Range: $minTemp%.1f°C to $maxTemp%.1f°C"
      }
      
      private def isValidTemperature(temp: Double): Boolean = {
        temp >= minTemp && temp <= maxTemp
      }
    }
    
    val controller = new TemperatureController(20.0, 24.0)
    
    println(controller.getStatus())
    println()
    
    controller.adjustTemperature()
    println(controller.getStatus())
    println()
    
    controller.adjustTemperature()
    println(controller.getStatus())
    println()
    
    controller.adjustTemperature()
    println()
    
    controller.setTarget(35.0)
  }
}
```

**Explanation:**
- All state is private (currentTemp, targetTemp)
- `require` validates constructor parameters
- Public getters provide read access
- Setter validates before modifying
- Private helper method encapsulates validation logic
- Cannot directly access or modify private fields
- Demonstrates proper encapsulation with validation

</details>

---

## Exercise 4: Complete OOP System

**Task:**  
Create a library management system combining all three OOP pillars.

**Requirements:**
- Abstract class `LibraryItem` with: title, id, isAvailable
- Abstract method `getType()`, concrete method `borrow()`, `returnItem()`
- Class `Book` extending LibraryItem with: author, pages
- Class `Magazine` extending LibraryItem with: issue, month
- Class `DVD` extending LibraryItem with: director, duration
- Class `Library` managing collection with encapsulation
- Methods: addItem, borrowItem, returnItem, listAvailable
- Demonstrate polymorphism with mixed collection

**Expected Output:**
```
Library: City Library

Added: The Great Gatsby (Book)
Added: National Geographic (Magazine)
Added: Inception (DVD)

Available items:
1. The Great Gatsby (Book) by F. Scott Fitzgerald
2. National Geographic (Magazine) - Issue: Jan 2024
3. Inception (DVD) directed by Christopher Nolan

Borrowed: The Great Gatsby

Available items:
1. National Geographic (Magazine) - Issue: Jan 2024
2. Inception (DVD) directed by Christopher Nolan

Returned: The Great Gatsby

Statistics:
Total Items: 3
Available: 3
Borrowed: 0
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object LibrarySystem {
  def main(args: Array[String]): Unit = {
    abstract class LibraryItem(val title: String, val id: String) {
      private var available: Boolean = true
      
      def isAvailable(): Boolean = available
      
      def getType(): String
      
      def getDetails(): String
      
      def borrow(): Boolean = {
        if (available) {
          available = false
          println(s"Borrowed: $title")
          true
        } else {
          println(s"$title is not available")
          false
        }
      }
      
      def returnItem(): Unit = {
        available = true
        println(s"Returned: $title")
      }
    }
    
    class Book(title: String, id: String, val author: String, val pages: Int)
      extends LibraryItem(title, id) {
      
      override def getType(): String = "Book"
      
      override def getDetails(): String = s"$title (Book) by $author"
    }
    
    class Magazine(title: String, id: String, val issue: String, val month: String)
      extends LibraryItem(title, id) {
      
      override def getType(): String = "Magazine"
      
      override def getDetails(): String = s"$title (Magazine) - Issue: $issue"
    }
    
    class DVD(title: String, id: String, val director: String, val duration: Int)
      extends LibraryItem(title, id) {
      
      override def getType(): String = "DVD"
      
      override def getDetails(): String = s"$title (DVD) directed by $director"
    }
    
    class Library(val name: String) {
      private var items: List[LibraryItem] = List()
      
      def addItem(item: LibraryItem): Unit = {
        items = items :+ item
        println(s"Added: ${item.title} (${item.getType()})")
      }
      
      def borrowItem(title: String): Boolean = {
        items.find(item => item.title == title && item.isAvailable()) match {
          case Some(item) => item.borrow()
          case None => 
            println(s"Item not found or not available: $title")
            false
        }
      }
      
      def returnItem(title: String): Boolean = {
        items.find(_.title == title) match {
          case Some(item) => 
            item.returnItem()
            true
          case None =>
            println(s"Item not found: $title")
            false
        }
      }
      
      def listAvailable(): Unit = {
        val available = items.filter(_.isAvailable())
        println("\nAvailable items:")
        available.zipWithIndex.foreach { case (item, index) =>
          println(s"${index + 1}. ${item.getDetails()}")
        }
      }
      
      def displayStatistics(): Unit = {
        val available = items.count(_.isAvailable())
        val borrowed = items.size - available
        println("\nStatistics:")
        println(s"Total Items: ${items.size}")
        println(s"Available: $available")
        println(s"Borrowed: $borrowed")
      }
    }
    
    val library = new Library("City Library")
    println(s"Library: ${library.name}\n")
    
    val book = new Book("The Great Gatsby", "B001", "F. Scott Fitzgerald", 180)
    val magazine = new Magazine("National Geographic", "M001", "Jan 2024", "January")
    val dvd = new DVD("Inception", "D001", "Christopher Nolan", 148)
    
    library.addItem(book)
    library.addItem(magazine)
    library.addItem(dvd)
    
    library.listAvailable()
    println()
    
    library.borrowItem("The Great Gatsby")
    library.listAvailable()
    println()
    
    library.returnItem("The Great Gatsby")
    
    library.displayStatistics()
  }
}
```

**Explanation:**
- **Inheritance:** Book, Magazine, DVD extend LibraryItem
- **Polymorphism:** Library stores mixed collection of LibraryItem
- **Encapsulation:** LibraryItem hides available field, Library hides items list
- Abstract methods force subclasses to implement getType and getDetails
- Pattern matching for safe item lookup
- Demonstrates all three OOP pillars working together

</details>

---

## Bonus Challenge (Optional)

**Task:**  
Create a bank account hierarchy with different account types and interest calculation.

**Requirements:**
- Abstract class `BankAccount` with balance encapsulation
- Abstract method `calculateInterest()`
- Concrete methods: deposit, withdraw, getBalance
- `SavingsAccount` with interest rate, minimum balance
- `CheckingAccount` with overdraft limit, transaction fee
- `InvestmentAccount` with risk level, variable returns
- `Bank` class managing multiple accounts polymorphically
- Demonstrate monthly interest calculation for all accounts

**Expected Output:**
```
Bank: First National Bank

Created Savings Account: SA001
Created Checking Account: CA001
Created Investment Account: IA001

Monthly Interest Applied:
SA001 (Savings): Interest of $50.00 applied. New balance: $10050.00
CA001 (Checking): No interest for checking accounts. Balance: $5000.00
IA001 (Investment): Interest of $300.00 applied. New balance: $20300.00

Total assets under management: $35350.00
```

<details>
<summary><strong>Click to see solution</strong></summary>

```scala
object BankSystem {
  def main(args: Array[String]): Unit = {
    abstract class BankAccount(val accountId: String, private var balance: Double) {
      def getBalance(): Double = balance
      
      protected def setBalance(newBalance: Double): Unit = {
        balance = newBalance
      }
      
      def deposit(amount: Double): Boolean = {
        if (amount > 0) {
          balance += amount
          true
        } else {
          false
        }
      }
      
      def withdraw(amount: Double): Boolean
      
      def calculateInterest(): Double
      
      def applyInterest(): Unit = {
        val interest = calculateInterest()
        if (interest > 0) {
          deposit(interest)
          println(f"$accountId (${getAccountType()}): Interest of $$${interest}%.2f applied. New balance: $$${getBalance()}%.2f")
        } else {
          println(f"$accountId (${getAccountType()}): No interest for this account type. Balance: $$${getBalance()}%.2f")
        }
      }
      
      def getAccountType(): String
    }
    
    class SavingsAccount(accountId: String, balance: Double, val interestRate: Double, val minBalance: Double)
      extends BankAccount(accountId, balance) {
      
      override def withdraw(amount: Double): Boolean = {
        if (amount > 0 && getBalance() - amount >= minBalance) {
          setBalance(getBalance() - amount)
          true
        } else {
          println(f"Cannot withdraw: would go below minimum balance of $$${minBalance}%.2f")
          false
        }
      }
      
      override def calculateInterest(): Double = getBalance() * interestRate / 12
      
      override def getAccountType(): String = "Savings"
    }
    
    class CheckingAccount(accountId: String, balance: Double, val overdraftLimit: Double)
      extends BankAccount(accountId, balance) {
      
      override def withdraw(amount: Double): Boolean = {
        if (amount > 0 && amount <= getBalance() + overdraftLimit) {
          setBalance(getBalance() - amount)
          true
        } else {
          println("Exceeds overdraft limit")
          false
        }
      }
      
      override def calculateInterest(): Double = 0.0  // No interest
      
      override def getAccountType(): String = "Checking"
    }
    
    class InvestmentAccount(accountId: String, balance: Double, val riskLevel: Int)
      extends BankAccount(accountId, balance) {
      
      override def withdraw(amount: Double): Boolean = {
        if (amount > 0 && amount <= getBalance()) {
          setBalance(getBalance() - amount)
          true
        } else {
          false
        }
      }
      
      override def calculateInterest(): Double = {
        val baseRate = 0.10  // 10% annual
        val riskMultiplier = 1 + (riskLevel * 0.5)
        getBalance() * baseRate * riskMultiplier / 12
      }
      
      override def getAccountType(): String = "Investment"
    }
    
    class Bank(val name: String) {
      private var accounts: List[BankAccount] = List()
      
      def createAccount(account: BankAccount): Unit = {
        accounts = accounts :+ account
        println(s"Created ${account.getAccountType()} Account: ${account.accountId}")
      }
      
      def applyMonthlyInterest(): Unit = {
        println("\nMonthly Interest Applied:")
        accounts.foreach(_.applyInterest())
      }
      
      def getTotalAssets(): Double = {
        accounts.map(_.getBalance()).sum
      }
    }
    
    val bank = new Bank("First National Bank")
    println(s"Bank: ${bank.name}\n")
    
    val savings = new SavingsAccount("SA001", 10000, 0.06, 1000)
    val checking = new CheckingAccount("CA001", 5000, 500)
    val investment = new InvestmentAccount("IA001", 20000, 2)
    
    bank.createAccount(savings)
    bank.createAccount(checking)
    bank.createAccount(investment)
    
    bank.applyMonthlyInterest()
    
    println(f"\nTotal assets under management: $$${bank.getTotalAssets()}%.2f")
  }
}
```

**Explanation:**
- Abstract `BankAccount` with protected `setBalance` for subclasses
- Each account type has different interest calculation
- Polymorphic collection in Bank class
- `applyMonthlyInterest` works with any BankAccount type
- Demonstrates encapsulation, inheritance, and polymorphism together
- Real-world example of OOP design

</details>

---

## Testing Your Programs

### Run and Verify:

1. **IntelliJ IDEA:**
   - Create Scala objects with code
   - Run each exercise
   - Verify output matches expected

2. **Experiment:**
   - Create additional subclasses
   - Test polymorphic collections
   - Verify encapsulation (try accessing private fields)
   - Test inheritance chain with `super`

---

## Self-Assessment Checklist

After completing these exercises, you should be able to:
- [ ] Create parent-child relationships with `extends`
- [ ] Override methods with `override` keyword
- [ ] Use `super` to call parent methods
- [ ] Create abstract classes with `abstract` keyword
- [ ] Define abstract methods (no implementation)
- [ ] Write polymorphic functions accepting parent type
- [ ] Store mixed types in collections
- [ ] Encapsulate state with private fields
- [ ] Provide controlled access with public methods
- [ ] Validate data in setters
- [ ] Combine all three OOP principles

---

## Common Mistakes to Avoid

1. **Forgetting override keyword:**
```scala
   class Dog extends Animal {
     def makeSound() = "Woof"  // Error! Missing override
     override def makeSound() = "Woof"  // Correct
   }
```

2. **Not calling parent constructor:**
```scala
   class Dog(name: String) extends Animal  // Error!
   class Dog(name: String) extends Animal(name)  // Correct
```

3. **Trying to instantiate abstract class:**
```scala
   val shape = new Shape()  // Error! Abstract class
   val circle = new Circle(5)  // Correct
```

4. **Accessing private fields from subclass:**
```scala
   class Parent {
     private var x = 10
   }
   class Child extends Parent {
     def test() = x  // Error! x is private
   }
```

---

## Key Concepts Review

**Remember:**
- Inheritance: code reuse through parent-child relationships
- Polymorphism: same interface, different behavior
- Abstract classes: blueprints with abstract methods
- Encapsulation: hide data, expose interface
- Override required for changing parent methods
- Super accesses parent implementation

---