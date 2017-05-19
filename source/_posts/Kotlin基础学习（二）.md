---
title: Kotlin基础学习（二）
date: 2017-05-19 10:04:34
tags:
- Kotlin
- Andrdoid
categories: [Kotlin]
---

Kotlin与Java的混编

<!-- more -->

# 直接转换

## Java转换为Kotlin

Intellij IDEA 支持直接将Java转换成Kotlin，键 Convert Java File to Kotlin File

## 注意class的调用

在 Java 或 Android 开发中，经常会直接调用一个类的 Class 文件。但是当你用上文介绍的转换方法去转换 **XXX.class** 这样的代码时，是无法直接转换的(也许未来会修复这个问题，但目前你扔需要手动修改)。在 M13 之前，Java 中的**XXX.class**对应 Kotlin 代码中的**JavaClass<XXX>**，而 M13 之后写法已被改为XXX::class.java。

## Android proguard 的坑

注：我们团队遇到过这样的一个坑，在 Android 开发的时候，如下代码会在混淆以后，发生异常
```Kotlin
var str = some?.s?.d ?: ""	
```
这段代码在正常debug模式编译运行完全正常，但是一旦执行混淆，就会发生所在函数被移除的现象。
但是如果改写为以下写法就能正常运行：
```Kotlin
var str = some?.s?.d ?: String()
```

同样的代码还有
```Kotlin
var list = some?.data?.list:mutableListof() 

//但是如下代码即使混淆后也是可以完全正常执行的

var s = some?.s ?: ""  
var s = some.d ?: ""
var list = some?.data?.list:klist  
var data = some?.data ?: return
```

## 开发 Android library 的建议
如果你是开发 Android library 程序，建议你不要使用 Kotlin 代码。因为作为 library，如果使用它的工程是纯 Java 完成的，引入后会额外增大 200k 左右大小，同时它有可能会造成某些情况下编译异常。

# 在 Kotlin 中调用 Java 代码

## 返回void的方法 

关键字 **unit**
如果一个 Java 方法返回 void，对应的在 Kotlin 代码中它将返回 Unit。
现在你只需要知道在Java 中返回为 void 的函数，在 Kotlin 中可以省略这个返回类型。

## 与 Kotlin 关键字冲突的处理

Java 有 static 关键字，在 Kotlin 中没有这个关键字，你需要使用@JvmStatic替代这个关键字。
同样，在 Kotlin 中也有很多的关键字是 Java 中是没有的。例如 in,is,data等。如果 Java 中使用了这些关键字，需要加上**反引号(`)转义来避免冲突**。例如
```Kotlin
// Java 代码中有个方法叫 is()
public void is(){
	//...
}

// 转换为 Kotlin 代码需要加反引号转义
fun `is`() {
   //...
}
```

# 在 Java 中调用 Kotlin 代码

## static 方法

在 Kotlin 中没有 static关键字,那么如果在 Java 代码中想要通过类名调用一个 Kotlin 类的方法，你需要给这个方法加入@JvmStatic注解。否则你必须通过对象调用这个方法。
```Kotlin
StringUtils.isEmpty("hello");  
StringUtils.INSTANCE.isEmpty2("hello");

object StringUtils {
    @JvmStatic fun isEmpty(str: String): Boolean {
        return "" == str
    }

    fun isEmpty2(str: String): Boolean {
        return "" == str
    }
}
```

如果你阅读 Kotlin 代码，应该经常看到这样一种写法。
```Kotlin
class StringUtils {
    companion object {
       fun isEmpty(str: String): Boolean {
	        return "" == str
	    }
    }
}
```

**companion object**表示外部类的一个**伴生对象**，你可以把他理解为外部类自动创建了一个对象作为自己的field。
与上面的类似，Java 在调用时，可以这样写：StringUtils.Companion. isEmpty();

## **包级别函数

与 Java 不同，Kotlin 允许函数独立存在，而不必依赖于某个类，这类函数我们称之为包级别函数(Package-Level Functions)。
为了兼容 Java，Kotlin 默认会将所有的包级别函数放在一个自动生成的叫**ExampleKt*的类中， 在 Java 中想要调用包级别函数时，需要通过这个类来调用。 
当然，也是可以自定义的，你只需要通过注解**@file:JvmName("Example")**即可将当前文件中的所有包级别函数放到一个自动生成的名为 Example 的类中。

## 空安全性

在 Java 中，如果你调用的 kotlin 方法参数声明了非空类型，如果你在 Java 代码中传入一个空值，将在运行时抛出NullPointerException。其内部原因在于 Kotlin 为每个非空类型加了**断言**，如果传入空值则会立刻抛出异常。 
同样，如果你使用 null 对象去调用一个 kotlin 方法，将会立刻抛出NullPointerException（就算是调用普通 java 方法也是一样会抛出 NullPointerException ）