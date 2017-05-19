---
title: Kotlin基础学习（一）
date: 2017-05-19 09:20:40
tags:
- Kotlin
- Andrdoid
categories: [Kotlin]
---
Kotlin基础学习(一)

<!-- more -->

# 介绍

[Kotlin](http://kotlinlang.org/) 是 JetBrains 在 2010 年推出的基于 JVM 的新编程语言。开发者称，设计它的目的是避免 Java 语言编程中的一些难题。比如：在 Kotlin 中类型系统控制了空指针引用，可以有效避免 Java 中常见的NullPointException。 
作为一个跨平台的语言，Kotlin 可以工作于任何 Java 的工作环境：服务器端的应用，移动应用（Android版），桌面应用程序。

# Kotlin的优势

相比于 Java，Kotlin 有着更好的语法结构，安全性和开发工具支持。
Kotlin 中没有基础类型，数组是定长的，泛型是安全的，即便运行时也是安全的。此外，该语言支持闭包，还可通过内联进行优化。不过，它不支持检查异常（Checked Exceptions），许多语言设计者认为这是它的瑕疵。不论如何，重要的是 Java 和 Kotlin 之间的互操作性：Kotlin 可以调用 Java，反之亦可。

# 准备工作

## 文档

[中文文档](http://www.kotlincn.net/docs/reference/basic-types.html)

## 安装插件

![Kotlin插件安装](http://upload-images.jianshu.io/upload_images/5214901-03ce0b19b4ac40cd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## Android Studio配置

![module buid.gradle](http://upload-images.jianshu.io/upload_images/5214901-f4e8473b1cde9a0a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![project build.gradle](http://upload-images.jianshu.io/upload_images/5214901-47aee66697c5a289.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 基础语法

## 变量

```Kotlin
fun main(args: Array<String>) {
    var quantity = 5           //变量
    val price: Double = 20.3   //常量
    val name: String = "大米"  //常量

    println("单价:$price")
    println("数量:$quantity")
    println("产品:$name 总计:${quantity * price}")
}
```

Kotlin语言声明一个变量使用关键字 **var**,声明一个常量用关键字 **val**, 声明时Kotlin语言可以自动推测出字段的类型。

## 语句

### in关键字的使用

判断一个对象是否在某一个区间内，可以使用in关键字
```Kotlin
//如果存在于区间(1,Y-1)，则打印OK
if (x in 1..y-1) 
  print("OK")

//如果x不存在于array中，则输出Out
if (x !in 0..array.lastIndex) 
  print("Out")

//打印1到5
for (x in 1..5) 
  print(x)

//遍历集合(类似于Java中的for(String name : names))
for (name in names)
  println(name)

//如果names集合中包含text对象则打印yes
if (text in names)
  print("yes")
  
//倒序迭代数字
for (i in 4 downTo 1) print(i)  //4,3,2,1

for (i in 4 downTo 1 step 2) print(i)  //4,2 

//不包括其结束元素的区间，可以使 until 函数
for (i in 1 until 10) { // i in [1, 10) 排除了 10
  println(i)
}
```

### when表达式

类似Java的**switch**，但是Kotlin更加智能，可以自动判断参数的类型并转换为响应的匹配值
```Kotlin
fun cases(obj: Any) { 
  when (obj) {
    1       -> print("第一项")
    "hello" -> print("这个是字符串hello")
    is Long -> print("这是一个Long类型数据")
    !is String -> print("这不是String类型的数据")
    else    -> print("else类似于Java中的default")
  }
}

//多个条件，用逗号隔开
when (x) {
0, 1 -> print("x == 0 or x == 1")
else -> print("otherwise")
}
```

### 智能类型推测 关键字 is

判断一个对象是否为一个类的实例，可以使用**is**关键字，与Java的instanceof类似，但在Kotlin中如果已经确定了一个对象的类型，可以在接下来的代码块中直接作为这个确定类型使用。
```Kotlin
fun getStringLength(obj: Any): Int? {
  if (obj is String) {
    // 做过类型判断以后，obj会被系统自动转换为String类型
    return obj.length 
  }

  //同时还可以使用!is，来取反
  if (obj !is String){
  }

  // 代码块外部的obj仍然是Any类型的引用
  return null
}
```

### 空值检测 关键字 ？

Kotlin是空指针安全的，也就意味着你不会再看空指针异常了。
```Kotlin
println(files?.size)    //只会在files不为空时执行

//当data不为空的时候，执行语句块
data?.let{
	//... 
}

//相反的，以下代码当data为空时才会执行
data?:let{
	//...
}
```

## 函数

### 函数的声明 关键字 fun

函数使用**fun**关键字声明
```Kotlin
//它接受一个String类型的参数，并返回一个String类型的值
fun say(str: String): String {
	return str
}

//同时，在 Kotlin 中，如果像这种简单的函数，可以简写为
fun say(str: String): String = str

//如果是返回Int类型，那么你甚至连返回类型都可以不写
fun getIntValue(value: Int) = value
```

### 函数的默认参数

使用默认参数来实现重载类似的功能
```Kotlin
fun say(str: String = "hello"): String = str

//多个参数也类似
fun say(firstName: String = "Tao",
		lastName: String = "Zhang"){
}
```

### 变参函数

同 Java 的变长参数一样，Kotlin 也支持变长参数，使用关键字 ** vararg **
```Kotlin
//在Java中，我们这么表示一个变长函数
public boolean hasEmpty(String... strArray){
	for (String str : strArray){
		if ("".equals(str) || str == null)
			return true;
	}
	return false;
}

//在Kotlin中，使用关键字vararg来表示
fun hasEmpty(vararg strArray: String?): Boolean{
	for (str in strArray){
		if ("".equals(str) || str == null)
			return true	
	}
	return false
}
```

### **拓展函数

你可以给父类添加一个方法，这个方法将可以在所有子类中使用
例如，在 Android 开发中，我们常常使用这样的扩展函数，然后可以在每一个Activity中直接使用toast()函数了。
```Kotlin
fun Activity.toast(message: CharSequence, duration: Int = Toast.LENGTH_SHORT) {
    Toast.makeText(this, message, duration).show()
}
```

### 将函数作为参数

Kotlin 中，可以将一个函数作为参数传递给另一个函数，叫作 **高阶函数**
```Kotlin
//上面的代码中，我们传入了一个无参的 body() 作为 lock() 的参数，
fun lock<T>(lock: Lock, body: () -> T ) : T { 
	lock.lock() 
	try { 
		return body() 
	} finally { 
		lock.unlock() 
	} 
} 
```

# 小结

```Kotlin
fun main(args: Array<String>) {

    val firstName: String = "Tao"
    val lastName: String? = "Zhang"

    println("my name is ${getName(firstName, lastName)}")
}

fun hasEmpty(vararg strArray: String?): Boolean {
    for (str in strArray) {
        str ?: return true
    }
    return false
}

fun getName(firstName: String?, lastName: String? = "unknow"): String {
    if (hasEmpty(firstName, lastName)) {
        lastName?.let { return@getName "${checkName(firstName)} $lastName" }
        firstName?.let { return@getName "$firstName ${checkName(lastName)}" }
    }
    return "$firstName $lastName"
}

fun checkName(name: String?): String = name ?: "unknow"
```



