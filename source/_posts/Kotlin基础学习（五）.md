---
title: Kotlin基础学习（五）
date: 2017-05-19 14:11:27
tags:
- Kotlin
- Android
categories: [Kotlin]
---
函数和闭包

<!-- more -->

# 函数

函数声明方式：关键字fun声明，如下代码创建了一个名为 say() 的函数，它接受一个 String 类型的参数，并返回一个 String 类型的值。
```Kotlin
fun say(str: String): String {
	return str
}
```

## Unit 

Unit表示的一个值的类型，这种类型对应于Java中的void。
如果一个函数是空函数，比如 Android 开发中的 TextWatch 接口，通常只会用到一个方法，但必须把所有方法都重写一遍，就可以通过这种方式来简写：
```Kotlin
editText.addTextChangedListener(object : TextWatcher {
    override fun afterTextChanged(s: Editable?) = Unit

    override fun beforeTextChanged(s: CharSequence?, start: Int, count: Int, after: Int) = Unit

    override fun onTextChanged(s: CharSequence?, start: Int, before: Int, count: Int) = Unit
})
```

## Nothing

如果一个函数不会返回，也就是说只要调用这个函数，那么在它返回之前程序肯定出错了，比如一定会抛出异常的函数。Nothing没有任何类型，表示永远不会存在的值。

# 复杂的特性

## 嵌套函数

Kotlin允许函数内声明函数。与内部类相似，内部函数可以访问外部函数的变量，一般情况下不推荐使用。使用场景一般是某些条件下触发的递归函数。
```Kotlin
fun function() {
  val valuesInTheOuterScope = "Kotlin is awesome!"
  
  fun theFunctionInside(int: Int = 10) {
    println(valuesInTheOuterScope)
    if (int >= 5) theFunctionInside(int - 1)
  }
  theFunctionInside()
}
```

## 运算符重载

```Kotlin
fun main(args: Array<String>) {
  for (i in 1..100 step 20) {
    print("$i ")
  }
}

//等同
for (i in 1.rangeTo(100) step 20) {
    print("$i ")
}
```
in 关键字 在编译后，会翻译为一个迭代器方法。
in 后面的**..**, 它表示一个封闭区间，其内部实现原理通过运算符的重载。
```Kotlin
//Int 类的源码中找到
public operator fun rangeTo(other: Int): IntRange
```
运算符重载需要使用关键字**operator**修饰，其余定于与函数相似。

## 中缀表达式

修饰符 **infix**
中缀表达式的定义：用一个字母或者单词来当运算符用。其不需要点和括号的方法调用。
```Kotlin
//step 就是 中缀表达式
public infix fun IntProgression.step(step: Int): IntProgression {
    checkStepIsPositive(step > 0, step)
    return IntProgression.fromClosedRange(first, last, if (this.step > 0) step else -step)
}
```

# 闭包

在Kotlin：函数、Lambda、if语句、for、when，都可以称之为闭包。但通常情况下，我们所说的闭包是 Lambda 表达式。

## 自执行闭包

自执行闭包就是在定义闭包的同时直接执行闭包，一般用于初始化上下文环境。 例如：
```Kotlin
{ x: Int, y: Int ->
    println("${x + y}")
}(1, 3)
```

# Lambda

## Lambda表达式

Lambda 表达式俗称匿名函数。Kotlin 的 Lambda表达式更“纯粹”一点， 因为它是真正把Lambda抽象为了一种类型，而 Java 8 的 Lambda 只是单方法匿名接口实现的语法糖罢了。
```Kotlin
val printMsg = { msg: String -> 
	println(msg) 
}

fun main(args: Array<String>) {
  printMsg.invoke("hello")
  或者
  printMsg("hello")
}
```
> Lambda 表达式还有非常多的语法糖，比如

> * 当参数只有一个的时候，声明中可以不用显示声明参数，在使用参数时可以用 **it** 来替代那个唯一的参数。
> * 当有多个用不到的参数时，可以用下划线来替代参数名(1.1以后的特性)，但是如果已经用下划线来省略参数时，是不能使用 it 来替代当前参数的。
> * Lambda 最后一条语句的执行结果表示这个 Lambda 的返回值。

**注意：闭包是不能有变长参数的 **

## 高阶函数

Lambda 表达式最大的特点是可以作为参数传递。当定义一个闭包作为参数的函数，称这个函数为高阶函数。
```Kotlin
fun main(args: Array<String>) {
    log("world", printMsg)
}

val printMsg = { str: String ->
    println(str)
}

val log = { str: String, printLog: (String) -> Unit ->
    printLog(str)
}
```
这个例子中，log 是一个接受一个 String 和一个以 String 为参数并返回 Unit 的 Lambda 表达式为参数的 Lambda 表达式。
读起来有点绕口，其实就是 log 有两个参数，一个str:String，一个printLog: (String) -> Unit。

## 内联函数

在使用高阶函数时，一定要知道内联函数这个东西。它可以大幅提升高阶函数的性能。
官方文档的描述是这样的：使用 高阶函数 在运行时会带来一些不利: 每个函数都是一个对象, 而且它还要捕获一个闭包,也就是,在函数体内部访问的那些外层变量. 内存占用(函数对象和类都会占用内存) 以及虚方法调用都会带来运行时的消耗.

但是也不是说所有的函数都要内联，因为一旦添加了**inline**修饰，在编译阶段，编译器将会把函数拆分，插入到调用出。如果一个 inline 函数是很大的，那他会大幅增加调用它的那个函数的体积。
