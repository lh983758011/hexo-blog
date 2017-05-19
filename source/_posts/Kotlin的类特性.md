---
title: Kotlin的类特性
date: 2017-05-19 10:29:15
tags:
- Kotlin
- Android
categories: [Kotlin]
---

Kotlin的类特性

<!-- more -->

# 构造函数

* Kotlin 的构造函数可以写在类头中，跟在类名后面，
* 如果有注解还需要加上关键字**constructor**。这种写法声明的构造函数，我们称之为主构造函数。

```Kotlin
class Person(private val name: String) {
    fun sayHello() {
        println("hello $name")
    }
}
```
* 在构造函数中声明的参数，它们默认属于类的公有字段，可以直接使用，如果你不希望别的类访问到这个变量，可以用**private**修饰。
* 在主构造函数中不能有任何代码实现，如果有额外的代码需要在构造方法中执行，你需要放到**init代码块**中执行。
同时，在本示例中由于需要更改 name 参数的值，我们将 val 改为 var,表明 name 参数是一个可改变的参数
```Kotlin
class Person(private var name: String) {
	
    init {
        name = "Zhang Tao"
    }

    internal fun sayHello() {
        println("hello $name")
    }
}
```
* 如果一个非抽象类没有声明任何(主或次)构造函数，它会有一个生成的不带参数的主构造函数。
* 构造函数的可见性是 public。如果你不希望你的类 有一个公有构造函数,你需要声明一个带有非默认可见性的空的主构造函数。 
* 另外，在 JVM 上,如果主构造函数的所有的参数都有默认值，编译器会生成一个额外的无参构造函数,它将使用默认值

#  次级构造函数

一个类可以有多个构造函数，只有主构造函数可以写在类头中，其他的次级构造函数（secondary consctructors）就需要写在类体中。
```Kotlin
class Person(private var name: String) {

    private var description: String? = null
    
    init {
        name = "Zhang Tao"
    }

    constructor(name: String, description: String) : this(name) {
        this.description = description
    }
    
    internal fun sayHello() {
        println("hello $name")
    }
}
```
次级构造函数调用了主构造函数，完成 name 的赋值。由于次级构造函数不能直接将参数转换为字段，所以需要手动声明一个 description 字段，并为 description 字段赋值。

# 修饰符

## open 修饰符

修饰类，可被继承。
Kotlin 默认会为每个变量和方法添加 final 修饰符。这么做的目的是为了程序运行的性能，其实在 Java 程序中，你也应该尽可能为每个类添加final 修饰符( 见 Effective Java 第四章 17 条)。 
为每个类加了final也就是说，在 Kotlin 中默认每个类都是不可被继承的。**如果你确定这个类是会被继承的，那么你需要给这个类添加 open 修饰符。**

##  internal 修饰符

Java 有三种访问修饰符，public/private/protected，还有一个默认的包级别访问权限没有修饰符。
在 Kotlin 中，默认的访问权限是 public。而多增加了一种访问修饰符叫 internal。它是模块级别的访问权限。
何为模块(module)，我们称被一起编译的一系列 Kotlin 文件为一个模块。在 IDEA 中可以很明确的看到一个 module 就是一个模块，当跨 module 的时候就无法访问另一个module 的 internal 变量或方法。

# 特殊类

## 枚举类

在 Kotlin 中，每个枚举常量都是一个对象。枚举常量用逗号分隔。 例如我们写一个枚举类 Programer。
```Kotlin
enum class Programer {
    JAVA, KOTLIN, C, CPP, ANDROID;
}
```
当它被编译成 class 后，将转为如下代码实际就是一个私有了构造函数的kotlin.Enum继承类。
```Kotlin
public final enum class Programer 
private constructor() : kotlin.Enum<Programer> {
    JAVA, KOTLIN, C, CPP, ANDROID;
}
```
Kotlin.Enum 类
```Kotlin
public abstract class Enum<E : Enum<E>>
(name: String, ordinal: Int): Comparable<E> {
    companion object {}

    /**
     * Returns the name of this enum constant,
     *  exactly as declared in its enum declaration.
     */
    public final val name: String

    /**
     * Returns the ordinal of this enumeration 
     * constant (its position in its enum 
     * declaration, where the initial constant
     * is assigned an ordinal of zero).
     */
    public final val ordinal: Int

    public override final fun compareTo(other: E): Int
}
```

## sealed 密封类

sealed 修饰的类称为密封类，用来表示受限的类层次结构。
例如当一个值为有限集中的 类型、而不能有任何其他类型时。
在某种意义上,他们是枚举类的扩展:枚举类型的值集合也是受限的，但每个枚举常量只存在一个实例,而密封类的一个子类可以有可包含状态的多个实例。

## data 数据类

data 修饰的类称之为数据类。它通常用在我们写的一些 POJO 类上。
当 data 修饰后，会自动将所有成员用operator声明，即为这些成员生成类似 Java 的 getter/setter 方法。

# 类的拓展

## 拓展方法

首先是一个fun关键字，紧接着是要扩展哪个类的类名，点方法名，然后是方法的声明和返回值以及方法体。
```Kotlin
fun Activity.toast(message: CharSequence, duration: Int = Toast.LENGTH_SHORT) {
    Toast.makeText(this, message, duration).show()
}
```

## 小心有坑

注意的是扩展方法是静态解析的，而并不是真正给类添加了这个方法。
```Kotlin
open class Animal{

}
class Dog : Animal()

object Main {
    fun Animal.bark() = "animal"

    fun Dog.bark() = "dog"

    fun Animal.printBark(anim: Animal){
        println(anim.bark())
    }

    @JvmStatic fun main(args: Array<String>) {
        Animal().printBark(Dog())
    }
}
```
最终的输出是 animal，而不是dog。
因为扩展方法是静态解析的，在添加扩展方法的时候类型为Animal，那么即便运行时传入了子类对象，也依旧会执行参数中声明时类型的方法。

## 强转与智能转换

在 Kotlin 中，用** is **来判断一个对象是否是某个类的实例，用** as **来做强转。
Kotlin 有一个很好的特性，叫 智能转换(smart cast)，在我之前的文章中也提到过。就是当已经确定一个对象的类型后，可以自动识别为这个类的对象，而不用再手动强转。
```Kotlin
fun main(args: Array<String>) {
	var animal: Animal? = null
    if (animal is Dog) {
    	//在这里你必须手动强转为Dog的对象
       animal.bark()
    }
}
```

例外
如果智能转换的对象是一个全局变量，这个变量可能在别的地方被改变赋值，所以你必须手动判断与转换它的类型。
```Kotlin
open class Animal {
}

class Dog : Animal() {
    fun bark() {
        println("animal")
    }
}

var animal: Animal? = null

fun main(args: Array<String>) {
    if (animal is Dog) {
    	//在这里你必须手动强转为Dog的对象
       (animal as Dog).bark()
    }
}
```

# 伴生对象

```Kotlin
class StringUtils {
    companion object {
       fun isEmpty(str: String): Boolean {
	        return "" == str
	    }
    }
}
```
由于 Kotlin 没有静态方法。在大多数情况下，官方建议是简单地使用 **包级 函数**。
如果你需要写一个可以无需用一个类的实例来调用、但需要访问类内部的函数(例如,工厂方法或单例),你可以把它写成一个用 companion修饰的对象内的方法。我们称companion修饰的对象为伴生对象。
将上面的代码编译后查看，实际上是编译器生成了一个public的内部对象。
```Kotlin
public final class StringUtils public constructor() {
    public companion object {
        public final fun isEmpty(str: kotlin.String): kotlin.Boolean { 
        /* compiled code */
        }
    }
}
```

# 单例类设计

伴生对象更多的用途是用来创建一个单例类。
如果只是简单的写，直接用伴生对象返回一个 val 修饰的外部类对象就可以了，但是更多的时候我们希望在类被调用的时候才去初始化他的对象。
以下代码将线程安全问题交给虚拟机在静态内部类加载时处理，是一种推荐的写法：
```Kotlin
class Single private constructor() {
    companion object {
        fun get():Single{
            return Holder.instance
        }
    }

    private object Holder {
        val instance = Single()
    }
}
```

# 动态代理

写多继承还是要根据场景来，正好今天跟朋友聊到他们项目重构的问题，我当时就说了一句：果然还是Kotlin好，原生支持动态代理。

朋友的一个 Android 项目，所有网络请求包括回调和参数全部封装在了一个 BaseActivity 中，然后随着项目越来越大，这一些网络请求方法想要抽出来，但又害怕牵连到线上的改动，我就推荐他用个动态代理来做，但是 Java 的动态代理又得要反射，又得要额外多写很多的代码方法，又是一个大改动。

反之看Kotlin的动态代理：
```Kotlin
interface Animal{
    fun bark()
}

class Dog :Animal {
    override fun bark() {
        println("Wang Wang")
    }
}

class Cat(animal: Animal) : Animal by animal {
}

fun main(args: Array<String>) {
   Cat(Dog()).bark()
}
```
这样，我们就很成功的让一只猫的叫声用狗去代理掉了，于是上面的main方法执行完后就变成了 Wang Wang。

# 伪多继承

Kotlin 的动态代理更多的是用在一种需要多继承的场景。
例如，还是之前我举的我朋友那个项目的例子，他们的问题在于，每个 BaseActivity 的子类，都会要请求不同的网络，可能A需要获取用户信息，B需要获取活动列表，C既需要活动列表也需要获取用户信息，D却只需要获取图片列表。

这样一个场景，使用一个代理类实现所有需要获取信息的接口方法。然后让不同的子类去实现所需的接口，请求统一交给代理类完成。这样不仅维护网络请求信息方便，而且每个类不会有额外多出来的方法防止新人接触项目的时候调用错请求方法。
```Kotlin
interface Animal{
    fun bark()
}

interface Food{
    fun eat()
}

class Delegate : Animal, Food {
    override fun eat() {
        println("mouse")
    }

    override fun bark() {
        println("Miao")
    }
}

class Cat(animal: Animal, food: Food) : Animal by animal, Food by food {
}

@JvmStatic fun main(args: Array<String>) {
    val delegate: Delegate = Delegate()
    Cat(delegate, delegate).bark()
}	
```
