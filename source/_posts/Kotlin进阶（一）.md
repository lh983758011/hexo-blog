---
title: Kotlin进阶（一）
date: 2017-05-19 16:49:18
tags:
- Kotlin
- 进阶
categories: [Kotlin]
---
Kotlin进阶（一）

<!-- more -->

# 反射

反射是这样的一组语言和库功能，它允许在运行时自省你的程序的结构。Kotlin 让语言中的函数和属性做为一等公民、并对其自省（即在运行时获悉一个
名称或者一个属性或函数的类型）与简单地使用函数式或响应式风格紧密相关。

> 注意：在 Java 平台上，使用反射功能所需的运行时组件作为单独的 JAR 文件件（kotlin-reflect.jar）分发。

## 类引用

最基本的反射功能就是获取Kotlin类的运行时引用。用 *类字面值* 语法：
```Kotlin
val c = MyClass::class 

//获取对象的类引用
val widget: Widget = ...
widget::class
```
该引用是[KClass](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.reflect/-k-class/index.html)类型的值。

## 函数引用

将函数作为一个值来传递，如传递给另一个函数
```Kotlin
fun isOdd(x : Int) = x % 2 != 0

//将函数isOdd传递给另一个函数
println(numbers.filter(::isOdd))
```
::isOdd是函数类型 (Int) -> Boolean的值。
