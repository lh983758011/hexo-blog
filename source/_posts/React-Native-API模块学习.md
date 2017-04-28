---
title: React Native API模块学习
date: 2017-04-28 15:13:05
tags:
- React Native
categories: [React Native]
---

React Native API模块学习

<!-- more -->

# NetInfo 网络信息

**NetInfo**模块可以监测客户端设备的联网状态，该模块通过异步的方式去监测联网状态。

```JavaScript
NetInfo.fetch().done((reach) => {
  //reach 网络连接信息
  console.log('Initial: ' + reach);
});

NetInfo.addEventListener(
  'change',
  handleFirstConnectivityChange
);

function handleFirstConnectivityChange(reach) {
  console.log('First change: ' + reach);
  NetInfo.removeEventListener(
    'change',
    handleFirstConnectivityChange
  );
}
```

## IOS 状态

1. none   设备没有联网

2. wifi     设备联网并且是连接的wifi网络，或者当前是iOS模拟器

3. cell      设备联网是通过连接Edge,3G,WiMax或者LET网络

4. unknown  该检测发生异常错误或者网络状态无从知道

## Android状态

1. NONE   设备没有网络连接

2. BLUETOOTH  蓝牙数据连接

3. DUMMY   虚拟数据连接

4. ETHERNET  以太网数据连接

5. MOBILE  手机移动网络数据连接

6. MOBILE_DUN  拨号移动网络数据连接

7. MOBILE_HIPRI  高权限的移动网络数据连接

8. MOBILE_MMS   彩信移动网络数据连接

9. MOBILE_SUPL   SUP网络数据连接

10. VPN   虚拟网络连接 ，最低支持Android API 21版本

11. WIFI   无线网络连接

12. WIMAX   wimax网络连接

13. UNKNOWN  未知网络数据连接

## 方法

* isConnectionExpensive(判断连接的网络是否计费,只适合Android)
 ```
 NetInfo.isConnectionExpensive((isConnectionExpensive) => {
 
  console.log('Connection is ' + (isConnectionExpensive ? 'Expensive' : 'Not Expensive'));
 
 });
 ```
 
* isConnected 判断是否有网络连接
 ```
 NetInfo.isConnected.fetch().done((isConnected) => {
 
  console.log('First, is ' + (isConnected ? 'online' : 'offline'));
 
 });
 
 function handleFirstConnectivityChange(isConnected) {
 
  console.log('Then, is ' + (isConnected ? 'online' : 'offline'));
 
  NetInfo.isConnected.removeEventListener(
 
    'change',
 
    handleFirstConnectivityChange
 
  );
 
 }
 
 NetInfo.isConnected.addEventListener(
 
  'change',
 
  handleFirstConnectivityChange
 
 );
 ```
* fetch() 静态方法，监测当前网络连接状态信息
 
# AsyncStorage 持久化存储模块

**AsyncStorage**是一个异步的，持久化的键-值存储系统，该模块的使用可以代替本地存储模块。
` 注意：官方推荐对AsyncStorage进行抽象封装再使用，因为AsyncStorage是操作全局的。`

该模块每个方法都会返回一个**Promise**对象。
` 注意：Promise对象，是用来传递异步操作的信息。它代表某个未来才会知道结果的事件（通常是一个异步操作），并且这个事件提供统一的API，可供进一步处理。
  Promise对象有两个特点：
  1、对象的状态不受外界影响。
  2、一旦状态改变就不会改变，任何时候都可以得到这个结果。
`






