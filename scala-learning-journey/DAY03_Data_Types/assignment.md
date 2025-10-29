# Day 03: Practice Assignment

## Overview
This assignment focuses on Scala's **data types**, **type hierarchy**, and **string interpolation**.  
You will practice declaring different types, formatting strings, and understanding Scala’s type system through hands-on exercises.

---

## Exercise 1: Type Exploration

**Task:**  
Write a program that declares variables of different numeric types and prints their values along with their type names.

**Expected Output:**
```

byte: 100 (Byte)
short: 1000 (Short)
int: 42 (Integer)
long: 9876543210 (Long)
float: 3.14 (Float)
double: 3.14159 (Double)

```

**Requirements:**
- Declare variables of type `Byte`, `Short`, `Int`, `Long`, `Float`, and `Double`
- Use `getClass.getSimpleName` to print the type name
- Use string interpolation (`s"..."`) for formatting

<details>
<summary>Show solution</summary>

```

object TypeExploration {
def main(args: Array[String]): Unit = {
val byteNum: Byte = 100
val shortNum: Short = 1000
val intNum: Int = 42
val longNum: Long = 9876543210L
val floatNum: Float = 3.14f
val doubleNum: Double = 3.14159

println(s"byte: $byteNum (${byteNum.getClass.getSimpleName})")
println(s"short: $shortNum (${shortNum.getClass.getSimpleName})")
println(s"int: $intNum (${intNum.getClass.getSimpleName})")
println(s"long: $longNum (${longNum.getClass.getSimpleName})")
println(s"float: $floatNum (${floatNum.getClass.getSimpleName})")
println(s"double: $doubleNum (${doubleNum.getClass.getSimpleName})")
}
}

```


**Explanation:**  
Each numeric type is explicitly declared.  
The `L` suffix represents a Long literal, and `f` represents a Float literal.  
`getClass.getSimpleName` is used to display the runtime type of each value.

</details>

---

## Exercise 2: String Interpolation Practice

**Task:**  
Write a program that stores product details (name, price, quantity) and displays them using Scala’s string interpolators.

**Expected Output:**
```

Product: Laptop
Price: $   999.50
Quantity: 3 units
Total Cost: $ 2998.50

```

**Requirements:**
- Use `s` interpolator for simple variable embedding
- Use `f` interpolator for formatted output
- Compute total cost using `price * quantity`
- Format price and total to two decimal places

<details>
<summary>Show solution</summary>

```

object ProductDisplay {
def main(args: Array[String]): Unit = {
val productName = "Laptop"
val price = 999.5
val quantity = 3

println(s"Product: $productName")
println(f"Price: $$ $price%8.2f")
println(s"Quantity: $quantity units")

val totalCost = price * quantity
println(f"Total Cost: $$ $totalCost%.2f")

}
}
```

**Explanation:**  
`f` interpolator allows numeric formatting, where `%8.2f` means width 8 with 2 decimals.  
`$$` is used to print a literal dollar sign.  
Arithmetic operations can be directly embedded in interpolated strings.

**Alternative Version:**
```

object ProductDisplay {
def main(args: Array[String]): Unit = {
val productName = "Laptop"
val price = 999.5
val quantity = 3
val totalCost = price * quantity

println("=" * 35)
println(s"Product: $productName")
println(f"Price: $$${price}%.2f")
println(s"Quantity: $quantity units")
println("-" * 35)
println(f"Total Cost: $$${totalCost}%.2f")
println("=" * 35)
}
}

```



</details>

---

## Exercise 3: Type Hierarchy Understanding

**Task:**  
Demonstrate Scala’s type hierarchy using `Any`, `AnyVal`, and `AnyRef`.

**Expected Output:**
```

Value: 42, is AnyVal: true, is AnyRef: false
Value: Hello, is AnyVal: false, is AnyRef: true
Value: true, is AnyVal: true, is AnyRef: false
Value: 3.14, is AnyVal: true, is AnyRef: false

```

**Requirements:**
- Create a list of mixed types using `List[Any]`
- Use `isInstanceOf` to check for `AnyVal` and `AnyRef`
- Print the result for each value

<details>
<summary>Show solution</summary>

```

object TypeHierarchyDemo {
  def main(args: Array[String]): Unit = {
    val mixedList: List[Any] = List(42, "Hello", true, 3.14)
    mixedList.foreach { value =>
      val isVal = value.isInstanceOf[AnyVal]
      val isRef = value.isInstanceOf[AnyRef]
      println(s"Value: $value, is AnyVal: $isVal, is AnyRef: $isRef")
    }
  }
}

```

**Explanation:**  
`Any` is the root of all Scala types.  
`AnyVal` covers value types (like `Int`, `Boolean`), and `AnyRef` covers reference types (like `String`, objects).

**Additional Example:**
```

object TypeRelations {
  def main(args: Array[String]): Unit = {
    val num = 42
    val text = "Hello"

    println(s"$num is Any: ${num.isInstanceOf[Any]}")
    println(s"$text is Any: ${text.isInstanceOf[Any]}")
    println(s"$num is AnyVal: ${num.isInstanceOf[AnyVal]}")
    println(s"$text is AnyRef: ${text.isInstanceOf[AnyRef]}")

    val nullString: String = null
    println(s"nullString is null: ${nullString == null}")
  }
}

```
</details>

---

## Bonus Challenge (Optional)

**Task:**  
Create a program that converts Celsius to Fahrenheit and Kelvin and displays all values neatly formatted.

**Formula:**  
- Fahrenheit = (Celsius × 9/5) + 32  
- Kelvin = Celsius + 273.15  

**Expected Output:**
```

# Temperature Converter

Celsius:     25.00°C
Fahrenheit:  77.00°F
Kelvin:     298.15K

```

<details>
<summary>Show solution</summary>

```

object TemperatureConverter {
  def main(args: Array[String]): Unit = {
    val celsius: Double = 25.0
    val fahrenheit = (celsius * 9.0 / 5.0) + 32
    val kelvin = celsius + 273.15
    println("Temperature Converter")
    println("=" * 21)
    println(f"Celsius:    $celsius%6.2f°C")
    println(f"Fahrenheit: $fahrenheit%6.2f°F")
    println(f"Kelvin:     $kelvin%6.2f K")
  }
}  

```


**Explanation:**  
The program uses string interpolation to display formatted numeric values with units.

**Alternative Version (Conversion Table):**
```

object TemperatureConverterAdvanced {
  def main(args: Array[String]): Unit = {
  val temps = List(0.0, 25.0, 37.0, 100.0)
    println("Temperature Conversion Table")
    println("=" * 50)
    println(f"${"Celsius"}%-12s ${"Fahrenheit"}%-12s ${"Kelvin"}%-12s")
    println("-" * 50)

    temps.foreach { c =>
      val f = (c * 9.0 / 5.0) + 32
      val k = c + 273.15
      println(f"$c%-12.2f $f%-12.2f $k%-12.2f")
    }
  println("=" * 50)
  }
}  

```

</details>

---

## Running Your Code

### Using Scala REPL
Enter:
```

scala

```
Then paste and execute your code.

### Using IntelliJ IDEA
1. Create a new Scala object.  
2. Paste your code.  
3. Right-click → **Run**.

### Using Command Line
```

scalac TypeExploration.scala   # Compile
scala TypeExploration          # Run

```

---

## Self-Assessment Checklist

- [ ] Declared numeric types: `Byte`, `Short`, `Int`, `Long`, `Float`, `Double`  
- [ ] Used correct suffixes (`L` for Long, `f` for Float)  
- [ ] Used string interpolation (`s`, `f`) properly  
- [ ] Formatted numeric output correctly  
- [ ] Understood differences between `Any`, `AnyVal`, and `AnyRef`  
- [ ] Used `isInstanceOf` for type checking  
- [ ] Practiced basic arithmetic and formatted output  

---

Once you’ve completed this practice set, move on to **Day 04** to explore variable declarations (`val` vs `var`), immutability, and deeper type usage.

---


