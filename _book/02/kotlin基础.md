
# kotlin基础


## 函数

```
fun max(a: Int, b: Int): Int {
    return if (a > b) a else b
}
```

* 函数的声明以关键字fun开始，函数名称紧随其后，接下来是括号括起来的参数列表，后面是返回类型，它们之间用一个冒号隔开。
* 在kotlin中，if是有结果值的表达式，和java中的三元运算符相似: (a > b) ? a : b。语句和表达式的区别在于，表达式有值，并且能作为另一个表达式的一部分使用。而语句总是包围着它的代码块中的顶层元素，并且没有自己的值。在java中，所有控制结构都是语句。而在kotlin中，除了循环(for、do和do/while)以外大多数控制结构都是表达式。
* 函数可以定义在文件的最外层，不需要把它放在类中。
* 可以省略每行代码结尾的分号。

#### 表达式函数体

* 函数体是由单个表达式构成的，可以用这个表达式作为完整的函数体。
```
fun max(a: Int, b: Int): Int = if (a > b) a else b
```
* 如果函数体写在花括号中，我们说这个函数有代码块体。如果它直接返回了一个表达式，它就有表达式体。
* 类型推导。每个变量和表达式都有类型，每个函数都有返回类型。但是对表达式体函数来说，编译器会分析作为函数体的表达式，并把它的类型作为函数的返回类型。对于有返回值的代码块体函数，必须显示地写出返回类型和return语句。
```
fun max(a: Int, b: Int) = if (a > b) a else b
```

#### 变量

* 在kotlin中以关键字开始，然后是变量名称，最后可以加上类型(不加也可以)。
* 如果变量没有初始化器，需要显式地指定它的类型。
* 可变变量: var，变量的值可以被改变，但它的类型是改变不了的。
```
var answer = 42
answer = "no answer" // 错误，类型不匹配
```
* 不可变变量: val，初始化之后不能再次赋值。如果编译器能确保只有唯一一条初始化语句被执行，可以根据条件使用不同的值来初始化。
```
val message: String
if (canPerformOperation()) {
    message = "successs";
} else {
    message = "fail";
}
```

#### 字符串模板

* 可以在字符串字面值中引用局部变量，只需要在变量名称前加上字符$。如: println("Hello, $name!")。
* 转义println("\$x")。
* 复杂的表达式，需要用花括号括起来。如: println("Hello, ${args[0]}!")。
* 还可以在花括号内嵌套双引号。如: println("Hello, ${if (args.size > 0) args[0] else "someone"}")。

## 类和属性

* 在类中声明属性
```
class Person(
    val name: String, // 只读属性，生成一个字段和一个简单的getter
    var isMarried: Boolean // 可写属性，一个字段、一个getter和一个setter
)
```
* 使用类
```
// kotlin
val person = Person("Bob", true) // 调用构造方法不需要关键字"new"
println(person.name) // 可以直接访问属性，但调用的是getter
println(person.isMarried)
// java
Person person = new Person("Bob", true);
System.out.println(person.getName());
System.out.println(person.isMarried());
```


#### 自定义访问器

```
class Rectangle(val height: Int, val width: Int) {
    val isSquare: Boolean // 属性
        get() {
            return height == width
        }
}
去掉花括号，简写成get() = height == width。
```

#### 目录和包

* package: 把类和函数的声明放在包中。
* import: 导入类和顶层函数，也可以在包名称后加上.*来导入特定包中定义的所有声明。
* 在java中，要把类放到和包结构相匹配的文件与目录结构中。在kotlin中，可以把多个类放在同一个文件中，文件的名字还可以随意选择，目录结构也没有任何限制。大多数情况下，应遵循java目录结构规则，尤其对于kotlin和java混用的项目。

## 枚举

* kotlin使用enum class两个关键字，enum是一个软关键字，只有当它出现在class前面时才有特殊意义，在其他地方可以把它当作普通的名称使用。
* 和java一样，枚举并不是值的列表，还可以给枚举类声明属性和方法。
```
enum class Color(val r: Int, val g: Int, val b: Int) { // 声明枚举常量的属性
    // 在每个常量创建的时候指定属性值
    RED(255, 0, 0), ORANGE(255, 165, 0),
    YELLOW(255, 255, 0), GREEN(0, 255, 0), BLUE(0, 0, 255),
    INDIGO(75, 0, 130), VIOLET(238, 130, 238); // 分号必须有
    
    // 给枚举类定义一个方法
    fun rgb() = (r * 256 + g) * 256 + b
}
println(Color.BLUE.rgb())
```
* 枚举常量用的声明构造方法和属性的语法与之前的常规类一样。
* 在枚举中定义任何方法，就要使用分号把枚举常量列表和方法定义分开。

## when表达式

* when是一个有返回值的表达式。
```
fun getMnemonic(color: Color) = 
    when (color) {
        Color.RED -> "Richard"
        Color.ORANGE -> "Of"
        Color.YELLOW -> "York"
        Color.GREEN -> "Gave"
        Color.BLUE -> "Battle"
        Color.INDIGO -> "In"
        Color.VIOLET -> "Vain"
    }
```
* 和java不一样，不需要再每个分支都写上break语句。也可以把多个值合并到同一个分支上，只需要逗号隔开。
```
fun getMnemonic(color: Color) = 
    when (color) {
        Color.RED, Color.ORANGE, Color.YELLOW -> "warm"
        Color.GREEN -> "neutral"
        Color.BLUE, Color.INDIGO, Color.VIOLET -> "cold"
    }
```
* 导入枚举常量后不用限定词就可以访问
```
import ch02.colors.Color
import ch02.colors.Color.*
fun getMnemonic(color: Color) = 
    when (color) {
        RED, ORANGE, YELLOW -> "warm"
        GREEN -> "neutral"
        BLUE, INDIGO, VIOLET -> "cold"
    }
```
* 在when结构中使用任意对象
```
fun mix(c1: Color, c2: Color) = 
    when (setOf(c1, c2)) {
        setOf(RED, YELLOW) -> ORANGE
        setOf(YELLOW, BLUE) -> GREEN
        setOf(BLUE, VIOLET) -> INDIGO
        else -> throw Exception("Dirty color")
    }
kotlin标准函数库中有一个setOf函数可以创建出一个Set。
```
* 使用不带参数的when
```
fun mixOptimized(c1: Color, c2: Color) = 
    when {
        (c1 == RED && c2 == YELLOW) ||
        (c1 == YELLOW && c2 == RED) -> ORANGE
        
        (c1 == YELLOW && c2 == BLUE) ||
        (c1 == BLUE && c2 == YELLOW) -> GREEN
        
        (c1 == BLUE && c2 == VIOLET) ||
        (c1 == VIOLET && c2 == BLUE) -> INDIGO
        
        else -> throw Exception("Dirty color")
    }
如果没有给when表达式提供参数，分支条件就是任意的布尔表达式。    
```
* 智能转换
```
在kotlin中，使用is来判断一个变量是否是某种类型，如果是，则可以把它当作这个类型使用。
if (e is Sum) {
    return eval(e.right) + eval(e.left)
}
使用as关键字来表示特定类型的显式转换
val n = e as Num
```
```
fun eval(e: Expr): Int = 
    if (e is Num) {
        e.value
    } else if (e is Sum) {
        eval(e.right) + eval(e.left)
    } else {
        throw IllegalArgumentException("Unknown expression")
    }
如果if分支中只有一个表达式，花括号是可以省略的。如果if分支是一个代码块，代码块中的最后一个表达式会被作为结果返回。
```
```
fun eval(e: Expr): Int = 
    when (e) {
        is Num -> e.value
        is Sum -> eval(e.right) + eval(e.left)
        else -> throw IllegalArgumentException("Unknown expression")
    }
when表达式不仅限于检查值是否相等，而且允许检查when实参值的类型。
```
* 代码块作为if和when的分支，最后一个表达式就是结果。

## for循环

* 区间: for(i in 1..100)，从1到100(包含100)。..语法不仅可以创建数字区间，还可以创建字符区间。
* 步长: for(i in 100 downTo 1 step 2)，步长为-2。
* 迭代不包含指定结束值的半闭合区间，可以使用until函数，如: for(x in 1 until 100)，从1到99。
* 迭代区间和数列、迭代集合(list、map等)。
```
val list = arrayListOf("10", "11", "1001")
for ((index, element) in list.withIndex()) {
    println("$index: $element")
}
```
* 使用in检查集合和区间的成员，或者它的逆运算!in，来检查这个值是否不在区间中。
```
fun isLetter(c: Char) = c in 'a'..'z' || c in 'A'..'Z'
fun isNotDigit(c: Char) = c !in '0'..'9'
```
* in和!in也适用于when表达式
```
fun recognize(c: Char) = when(c) {
    in '0'..'9' -> "It's a digit!"
    in 'a'..'z', in 'A'..'Z' -> "It's a letter!"
    else -> "I don't know..."
}
```
* 实现了接口java.lang.Comparable，就能创建这种类型的对象的区间。如果这样的区间，并不能列举出区间内所有的对象，但仍然可以使用in运算符检查另一个对象是否属于这个区间。
```
println("Kotlin" in "Java".."Scala") // true
这里是字符串按照字母表顺序进行比较，因为String就是这样实现Comparable接口的。
```
* in检查也适用于集合
```
println("Kotlin" in setOf("Java", "Scala")) // false
```

## 异常

* 不必使用new关键字来创建异常实例。
* throw结构是一个表达式。
```
val percentage = 
    if (number in 0..100)
        number
    else
        throw IllegalArgumentException(
        "A percentage value must be between 0 and 100: $number")
抛出异常时，percentage变量不会被初始化。
```
* 函数声明时不必写上throws子句。
```
fun readNumber(reader: BufferedReader): Int { // 不必显式地指定这个函数可能抛出的异常
    try {
        val line = reader.readLine()
        return Integer.parseInt(line)
    } catch (e: NumberFormatException) {
        return null
    } finally {
        reader.close()
    }
}
```
* 和其它许多现代JVM语言一样，kotlin并不区分受检异常和未受检异常。

#### try作为表达式

* kotlin中try关键字就像if和when一样，引入了一个表达式，可以把它的值赋给一个变量。如果其主体包含多个表达式，那么整个try表达式的值就是最后一个表达式的值。
```
fun readNumber(reader: BufferedReader) {
    val number = try {
        Integer.parseInt(reader.readLine())
    } catch (e: NumberFormatException) {
        return
    } 
    
    println(number) // 抛出异常后，此处不会执行
}
fun readNumber(reader: BufferedReader) {
    val number = try {
        Integer.parseInt(reader.readLine())
    } catch (e: NumberFormatException) {
        null
    } 
    
    println(number) // 抛出异常后，打印null
}
```