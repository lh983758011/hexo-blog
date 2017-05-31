---
title: Kotlin基础学习
date: 2017-05-22 16:08:05
tags:
- Kotlin
categories: [Kotlin]
---
基础学习

<!-- more -->

# 基础学习

* 遍历map
```Kotlin
for ((k, v) in map) {
  println("$k -> $v")
}
//k 、v 可以改成任意名字
```

* 只读List
```Kotlin
val list = listOf("a", "b", "c")
```

* 只读Map
```Kotlin
val map = mapOf("a" to 1, "b" to 2, "c" to 3)
```

* 访问Map
```Kotlin
println(map["key"])
map["key"] = value
```

* 延迟属性
```Kotlin
val p: String by lazy {
// 计算该字符串
}
```

* 创建单例
```Kotlin
object Resource {
val name = "Name"
}
```

* ? 判断 not null 或者 null
```Kotlin
//if not null 缩写
val files = File("Test").listFiles()
println(files?.size)

//if not null and else 缩写
val files = File("Test").listFiles()
println(files?.size ?: "empty")

//if null
val data = ……
val email = data["email"] ?: throw IllegalStateException("Email is missing!")

//if not null 执行代码
val data = ……
data?.let {
…… // 代码会执⾏到此处, 假如data不为null
}
```

* try/catch 表达式
```Kotlin
fun test() {
  val result = try {
    count()
  } catch (e: ArithmeticException) {
    throw IllegalStateException(e)
  } 
  // 使用 result
}
```

* 对一个对象调用多个方法
```Kotlin
class Turtle {
  fun penDown()
  fun penUp()
  fun turn(degrees: Double)
  fun forward(pixels: Double)
} 
val myTurtle = Turtle()
with(myTurtle) { // 画⼀个 100 像素的正⽅形
  penDown()
  for(i in 1..4) {
    forward(100.0)
    turn(90.0)
  }
  penUp()
}
```

* 数字字面值中的下划线（自1.1 起）,让数字常量更加易读
```Kotlin
val oneMillion = 1_000_000
val creditCardNumber = 1234_5678_9012_3456L
val socialSecurityNumber = 999_99_9999L
val hexBytes = 0xFF_EC_DE_5E
val bytes = 0b11010010_01101001_10010100_10010010
```

* 显示转换
```Kotlin
toByte(): Byte
toShort(): Short
toInt(): Int	
toLong(): Long
toFloat(): Float
toDouble(): Double
toChar(): Char
```

* 运算
只可用**中缀方式**调用命名函数,完整的位运算列表（只⽤于 Int 和 Long ）
```Kotlin
val x = (1 shl 2) and 0x000FF000

shl(bits) ‒ 有符号左移 (Java 的 << )
shr(bits) ‒ 有符号右移 (Java 的 >> )
ushr(bits) ‒ ⽆符号右移 (Java 的 >>> )
and(bits) ‒ 位与
or(bits) ‒ 位或
xor(bits) ‒ 位异或
inv() ‒ 位⾮
```

* 数组
**Array** 表示数组，用库函数**arrayOf()** 创建一个数组并传递数据，
**arrayOfNulls()** 创建一个空数组
ByteArray 、ShortArray 、IntArray 等等。这些类和 Array 并没有继承关系，但是 它
们有同样的方法属性集
```Kotlin
class Array<T> private constructor() {
val size: Int
operator fun get(index: Int): T
operator fun set(index: Int, value: T): Unit
operator fun iterator(): Iterator<T>
// ……
}
```

* 标记
在 Kotlin 中任何表达式都可以用**标签（label）**来标记。标签的格式为标识符后跟 **@** 符号，例如：abc@ 、fooBar@ 都是有效的标签。要为一
个表达式加标签，我们只要在其前加标签即可
```Kotlin
loop@ for (i in 1..100) {
  for (j in 1..100) {
    if (……) 
	  break@loop
  }
}
```
标签处返回
```Kotlin
fun foo() {
  ints.forEach {
    if (it == 0) return
    print(it)
  }
}
//这个 return 表达式从最直接包围它的函数即 foo 中返回。（注意，这种非局部的返回只支持传给内联函数的 lambda 表达式。）如果我们需要从
//lambda 表达式中返回，我们必须给它加标签并用以限制 return。

fun foo() {
  ints.forEach lit@ {
    if (it == 0) return@lit
    print(it)
  }
}
//现在，它只会从 lambda 表达式中返回。通常情况下使用隐式标签更方便。
//该标签与接受该 lambda 的函数同名。
//或者，我们用一个匿名函数替代 lambda 表达式。匿名函数内部的 return 语句将从该匿名函数自身返回

//标签是lambda的函数名。
fun foo() {
  ints.forEach {
    if (it == 0) return@forEach
    print(it)
  }
}

//匿名函数
fun foo() {
  ints.forEach(fun(value: Int) {
    if (value == 0) return
    print(value)
  })
}

//当要返一个回值的时候，解析器优先选用标签限制的 return，意为“从标签 @a 返回 1”，而不是“返回一个标签标注的表达式 (@a 1) ”。
return@a 1
```

# 类

## 构造函数

* 主构造函数
在 Kotlin 中的一个类可以有一个主构造函数和一个或多个次构造函数。主构造函数是类头的一部分：它跟在类名（和可选的类型参数）后
```Kotlin
class Person constructor(firstName: String) {

} 	
```
如果主构造函数没有任何注解或者可见性修饰符，可以省略这个 constructor 关键字。
如果构造函数有注解或可见性修饰符，这个 constructor 关键字是必需的，并且 这些修饰符在它前面
```Kotlin
class Person(firstName: String) {

}

class Customer public @Inject constructor(name: String) 
{ 

}
```
主构造函数不能包含任何的代码。初始化的代码可以放 到以**init**关键字作为前缀的初始化块（initializer blocks）中：
```Kotlin
class Customer(name: String) {
  init {
    logger.info("Customer initialized with value ${name}")
  }
}
```
与普通属性一样，主构造函数中声明的属性可以是 可变的（var）或只读的（val）。

* 次构造函数
类也可以声明前缀有 constructor** 的次构造函数**：
```Kotlin
class Person {
  constructor(parent: Person) {
    parent.children.add(this)
  }
}
```
> 如果类有一个主构造函数，每个次构造函数需要委托给主构造函数，可以直接委托或者通过别的次构造函数间接委托。委托到同一个类的另一个构造函数用**this**关键字即可：

```Kotlin
class Person(val name: String) {
  constructor(name: String, parent: Person) : this(name) {
    parent.children.add(this)
  }
}
```
如果一个非抽象类没有声明任何（主或次）构造函数，它会有一个生成的不带参数的主构造函数。构造函数的可见性是 public。如果你不希望你的类有一
个公有构造函数，你需要声明一个带有非默认可见性的空的主构造函数
```Kotlin
class DontCreateMe private constructor () {
}
```

* 创建类实例
```Kotlin
val invoice = Invoice()
val customer = Customer("Joe Smith")
```
> 注意 Kotlin 并没有 **new** 关键字.

* 类成员
 - 构造函数和初始化块
 - 函数
 - 属性
 - 嵌套类和内部类
 - 对象声明

## 继承

> 在 Kotlin 中所有类都有一个共同的超类 **Any** ，这对于没有超类型声明的类是默认超类：
Any 不是 java.lang.Object ；尤其是，它除了 equals() 、hashCode() 和 toString() 外没有任何成员。

如果类没有主构造函数，那么每个次构造函数必须 使用 **super** 关键字初始化其基类型，或委托给另一个构造函数做到这一点。注意，在这种情况下，不同
的次构造函数可以调用基类型的不同的构造函数.
```Kotlin
class MyView : View {
  constructor(ctx: Context) : super(ctx)
  constructor(ctx: Context, attrs: AttributeSet) : super(ctx, attrs)
}
```

类上的 **open** 标注与 Java 中 **final** 相反，它允许其他类从这个类继承。默认情况下，在 Kotlin 中所有的类都是 **final**。

* 覆盖方法
```Kotlin
open class Base {
  open fun v() {}
    fun nv() {}
  }
class Derived() : Base() {
  override fun v() {}
}
```
Derived.v()函数上必须加上**override**标注。如果没写，编译器将会报错。如果函数没有标注**open**如 Base.nv() ，则子类中不允许定义相同签名的函
数，不论加不加 **override**。在一个**final**类中（没有用**open**标注的类），开放成员是禁止的。

标记为**override**的成员本来是开放的，也就是说，它可以在子类中覆盖。如果你想禁止再次覆盖，使用**final**关键字。
```Kotlin
open class AnotherDerived() : Base() {
  final override fun v() {}
}
```

* 覆盖属性
属性覆盖与方法覆盖类似；在超类中声明然后在派生类中重新声明的属性必须以**override**开头，并且它们必须具有兼容的类型。每个声明的属性可以由
具有初始化器的属性或者具有**getter**方法的属性覆盖。
```Kotlin
open class Foo {
  open val x: Int get { …… }
} 
class Bar1 : Foo() {
  override val x: Int = ……
}
```
**var**属性覆盖一个**val**属性，但反之则不行。因为一个**val**属性本质上声明了一个**getter**方法，而将其覆盖为**var**只
是在子类中额外声明一个**setter**方法。

> 请注意，你可以在主构造函数中使用 override 关键字作为属性声明的一部分。

* 覆盖规则
如果一个类从它的直接超类继承相同成员的多个实现，它必须覆盖这个成员并提供其自己的实现（也许用继承来
的其中之一）。为了表示采用从哪个超类型继承的实现，我们使用由尖括号中超类型名限定的**super**，如**super<Base>**
```Kotlin
open class A {
  open fun f() { print("A") }
  fun a() { print("a") }
}
 
interface B {
 fun f() { print("B") } // 接口成员默认就是“open”的
 fun b() { print("b") }
} 

class C() : A(), B {
  // 编译器要求覆盖 f()：
  override fun f() {
    super<A>.f() // 调用 A.f()
    super<B>.f() // 调用 B.f()
  }
}
```

## 抽象类

类和其中某些成员可以声明为**abstract**。
可以用一个抽象成员覆盖一个非抽象的开放成员。
```Kotlin
open class Base {
  open fun f() {}
} 

abstract class Derived : Base() {
  override abstract fun f()
}
```

## 伴生对象

> 与 Java 或 C# 不同，在 Kotlin 中类没有静态方法。在大多数情况下，它建议简单地使用**包级函数**。

在类内声明了一个伴生对象，你就可以使用像在 Java/C# 中调用静态方法相同的语法来调用其成员，只使用类名作为限定符。

# 属性和字段

## 声明属性

属性可以用关键字**var**声明为可变的，否则使用只读关键字**val**

## Getter和Setter

```Kotlin
var <propertyName>[: <PropertyType>] [= <property_initializer>]
  [<getter>]
  [<setter>]
  
//实例
var stringRepresentation: String
  get() = this.toString()
  set(value) {
    setDataFromString(value) // 解析字符串并赋值给其他属性
  }
```

改变一个范围器的可见性或者对其注解，但是不需要改变默认的实现，可以定义访问器而不定义其实现。
```Kotlin
var setterVisibility: String = "abc"
  private set // 此 setter 是私有的并且有默认实现
  
var setterWithAnnotation: Any? = null
  @Inject set //  用Inject 注解此 setter
```

* 幕后字段
用标识符**field**，并且只能用在属性的访问器内。
```Kotlin
var counter = 0 // 此初始器值直接写⼊到幕后字段
  set(value) {
    if (value >= 0)
      field = value
  }
```
如果属性至少一个访问器使用默认实现，或者自定义访问器通过**field**引用幕后字段，将会为该属性生成一个幕后字段。

* 幕后属性
```Kotlin
private var _table: Map<String, Int>? = null
public val table: Map<String, Int>
  get() {
    if (_table == null) {
      _table = HashMap() // 类型参数已推断出
    }
	return _table ?: throw AssertionError("Set to null by another thread")
  }
```

## 编译期常量

用关键字**const**修饰，并且需满足一下要求
- 位于顶层或者是 object 的⼀个成员
- 用String 或原生类型值初始化
- 没有自定义 getter
```Kotlin
const val SUBSYSTEM_DEPRECATED: String = "This subsystem is deprecated"
```

## 惰性初始化属性

用修饰符**lateinit**
> 该修饰符只能用于在类体中（不是在主构造函数中）声明的 var 属性，并且仅当该属性没有自定义 getter 或 setter 时。该属性必须是非空类型，并且不能是原生类型。

# 接口

> Kotlin 的接口与 Java 8 类似，既包含抽象方法的声明，也包含实现。与抽象类不同的是，接口无法保存状态。它可以有属性但必须声明为抽象或提供访问器实现，在接口中声明的属性要么是抽象的，要么提供访问器的实现。
一个类或者对象可以实现一个或者多个接口。

用关键字**interface**定义接口。
```Kotlin
interface MyInterface {
  fun bar()
  fun foo() {
    // 可选的方法体
  }
}

//实现接口
class Child : MyInterface {
  override fun bar() {
    // 方法体
  }
}
```







