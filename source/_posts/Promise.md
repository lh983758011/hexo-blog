---
title: Promise
date: 2017-05-03 14:18:49
tags:
- React Native
categories: [React Native]
---
Promise

<!-- more -->

# 出现情况

假设现在一个日常开发会遇到这样一个需求：多个接口异步请求，第二个接口依赖于第一个接口执行完毕之后才能利用数据进行一系列操作。一般会这样写：

```JavaScript
A.fetchData({
                url: 'http://......',
                success: function (data) {
                    A.fetchData({
                        // 要在第一个请求成功后才可以执行下一步
                        url: 'http://......',
                        success: function (data) {
                             // ......
                        }
                    });
                }
            });
```

这样写没问题，但是有两个缺点：
1、当有多个操作的时候，会导致多个回调函数嵌套，不够美观
2、如果有几个操作没有前后顺序之分时，例如上面的后一个请求不依赖与前一个请求的返回结果的时候，同样也需要等待上一个操作完成再实行下一个操作。

从ES6开始，Promise对象可以解决上述问题。

# 什么是Promise

一个Promise可以理解为一次将要执行的操作，使用了Promise对象之后可以用一种链式调用的方式组织代码，让代码更加直观。

实现原理：使用setImmediate(fn)来实现异步调用。

## resolve和reject

```JavaScript
function helloWorld (ready) {
    return new Promise(function (resolve, reject) {
        if (ready) {
            resolve("Hello World!");
        } else {
            reject("Good bye!");
        }
    });
  }

helloWorld(true).then(function (message) {
        alert(message);
	}, function (error) {
        alert(error);
    });
```

在Promise对象当中有两个重要的方法 -- **resolve**和**reject**。

**resolve**方法可以使Promise对象的状态改变成功，同时传递一个参数用于后续成功后的操作。

**reject**方法则是将Promise对象的状态改变为失败，同时将错误的信息传递到后续错误处理的操作。

## then

Promise有三种状态：
1. Fulfilled：可以理解为成功的状态；
2. Rejected: 可以理解为失败的状态；
3. Pending: 既不是成功也不是失败的状态，可以理解为Promise对象实例时候的初始状态；

Promise对象上的**then**方法负责添加针对已完成和拒绝状态下的处理函数。** then 方法会返回另一个Promise对象 **，以便于形成Promise管道。这种返回方式支持开发人员把异步操作串联起来，then(resolveHandle, rejectHandle)
resolveHandle回调函数在Promise对象进入完成状态时触发，并传递结果；
rejectHandle回调函数会在拒绝状态下调用。

## catch

catch 方法是 then(onFulfilled, onRejected) 方法当中 onRejected 函数的一个简单的写法，也就是说可以写成 then(fn).catch(fn)，相当于 then(fn).then(null, fn)。使用 catch 的写法比一般的写法更加清晰明确。

# 其他情况

## 返回一个具体的值或者是 undefined

```JavaScript
getUserByName('nolan').then(fcuntion (user) {
        if (inMemoryCache[user.id]) {
            return inMemoryCache[user.id];  
            // returning a value!
        }
        return inMemoryCache[user.id];
         // returning a promise
    }).then(function (userAccount) {
        // I got a user account
    })
```

如果不调用return语句的话，javaScript里的函数会返回 undefined 。这也就意味着在你想返回一些值的时候，不显式调用return会产生一些副作用。

出于上述原因，养成了一个个人习惯就是在then方法内部永远显式的调用return或者throw。我也推荐你这样做。

## 抛出一个错误

说到throw，这又体现了promise的功能强大。在用户退出的情况下，我们的代码中会采用抛出异常的方式进行处理：
```JavaScript
 getUserByName('nolan').then(function (user) {
      if (user.isLoggedOut()) {
        throw new Error('user logged out!'); 
        // throwing a error!
      }
      if (inMemoryCache[user.id]) {
        return inMemoryCache[user.id];      
         // returning a value!
      }
      return getUserAccountById(user.id);   
       // returning a promise!
    }).then(function (userAccount) {
      // I got a user account!
    }).catch(function (err) {
      // Boo, I got an error!
    });
```






