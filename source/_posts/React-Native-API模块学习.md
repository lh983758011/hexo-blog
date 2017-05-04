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

```JavaScript
async _loadInitialState(){
       try{
          var value=await AsyncStorage.getItem(STORAGE_KEY_ONE);
          if(value!=null){
            this._appendMessage('从存储中获取到数据为:'+value);
          }else{
            this._appendMessage('存储中无数据,初始化为空数据');
          }
       }catch(error){
            this._appendMessage('AsyncStorage错误'+error.message);
       }
  }
//进行储存数据_ONE
async _saveValue_One(){
      try{
         await AsyncStorage.setItem(STORAGE_KEY_ONE,'我是老王');
         this._appendMessage('保存到存储的数据为:'+'我是老王');
      }catch(error){
         this._appendMessage('AsyncStorage错误'+error.message);
      }
  }
//进行存储数据删除_ONE
async _removeValue_One(){
      try{
         await AsyncStorage.removeItem(STORAGE_KEY_ONE);
         this._appendMessage('数据删除成功...');
      }catch(error){
         this._appendMessage('AsyncStorage错误'+error.message);
      }
  }
```

# Dimensions屏幕宽高

获取屏幕的宽高

```JavaScript
<Text style={styles.instructions}>
  当前屏幕宽度:+{Dimensions.get('window').width};
</Text>
<Text style={styles.instructions}>
  当前屏幕高度:'+{Dimensions.get('window').height};
</Text>
```

# PixelRatio设备像素密度

1. get()  static静态方法，进行返回屏幕的像素密度。一些例子如下:
 > PixelRatio.get()==1     mdpi  Android设备(160 dpi)
 > PixelRatio.get()==1.5  hdpi  Android设备(240 dpi)
 > PixelRatio.get()==2  iPhone4,4S,iPhone 5,5C,5S,iPhone 6,xhdpi Android设备(320 dpi)
 > PixelRatio.get()==3  iPhone6 Plus,xxhdpi Android设备(480 dpi)
 > PixelRatio.get()==3.5  Nexus 6

2. getFontScale()  static 静态方法  进行获取文字大小的缩放比例，这个比例可以用来计算字体的绝对大小。所有很多元素可以用这个结果进行计算大小。如果没有设置字体缩放大小，那么该直接回返回设备像素密度。

 目前:该方法现在只是在Android平台实现了，在Android平台上面我们可以去设备Settings->Display->Font Size(查询字体大小)。在iOS平台上面总会返回默认的像素密度。

3. getPixelSizeForLayoutSize(layoutSize:number)  static 静态方法， 进行把dp转换成像素px,该会返回一个整形的数值

4. roundToNearestPixel(layoutSize:number)  static 静态方法，

5. startDetecting() 静态方法，该方法现在在移动设备上面暂时不支持

# AppRegistry运行入口

1. registerConfig(config:Array<AppConfig>)  static 静态方法, 进行注册配置信息

2. registerComponent(appKey:string,getComponentFunc:ComponentProvider)  static静态方法，进行注册组件

3. registerRunnable(appKey:string,func:Function)  static静态方法 ，进行注册线程

4. registerAppKeys()  static静态方法，进行获取所有组件的keys值

5. runApplication(appKey:string,appParameters:any)  static静态方法, 进行运行应用

6. unmountApplicationComponentAtRootTag()  static静态方法，结束应用

# Linking
Linking模块提供了Android和IOS双平台通用的接口，进行处理APP进入和传出的链接。

1. addEventLinstener(type,handler )  static 静态方法,适合iOS平台 。添加监听Linking事件变化的方法，第一个参数是事件类型url,第二个参数为事件处理方法。

2. removeEventListener(type,handler)  static 静态方法,适合iOS平台。移除监听Linking事件变化，第一个参数是事件类型url，第二个参数为事件处理方法。

3. openURL()  static静态方法, 进行使用设备中已经安装的应用打开指定的URL。当然你还可以使用其他类型的URLs，例如地址信息位置(举例:"geo:37.484847,-122.148386"),联系人或者其他可以被安装的应用打开的URL都可以。

 **[注意]**.如果系统不能处理打开传入的URL，那么该方法会返回失败。如果你传入的不是一个http(s)URL，最好的方法就是使用 {@code canOpenURL} 进行检查一下。

 **[注意]**.对于Web  URL,连接协议头("http://","https://")不能省略

4. canOpenURL(url)  static静态方法,该用来检查判断传入的URL是否可以被已经安装的应用打开。

 **[注意]**.对于Web  URL,连接协议头("http://","https://")不能省略

 **[注意]**.在iOS 9开始的系统中，你需要在Info.plist中添加LSApplicationQueriesSchemes字段。

5. getInitialURL()  static方法，如果当前的应用是被一个外部传入的URL调起打开的，那么该方法返回一个链接地址，否则会返回null

实例
```JavaScript
import React, {
  AppRegistry,
  Component,
  StyleSheet,
  Text,
  View,
  Linking,
  TouchableHighlight,
} from 'react-native';
class CustomButton extends React.Component {
  constructor(props){
    super(props);
  }
  propTypes: {
    url: React.PropTypes.string,
  }
  render() {
    return (
      <TouchableHighlight
        style={styles.button}
        underlayColor="#a5a5a5"
        onPress={()=>Linking.canOpenURL(this.props.url).then(supported => {
           if (supported) {
               Linking.openURL(this.props.url);
           } else {
              console.log('无法打开该URI: ' + this.props.url);
           }
        })}>
        <Text style={styles.buttonText}>{this.props.text}</Text>
      </TouchableHighlight>
    );
  }
}
class LinkingDemo extends Component {
  componentDidMount() {
   var url = Linking.getInitialURL().then((url) => {
    if (url) {
      console.log('捕捉的URL地址为: ' + url);
    }
   }).catch(err => console.error('错误信息为:', err));
 }
  render() {
    return (
      <View>
         <CustomButton url={'http://www.lcode.org'}  text="点击打开http网页"/>
         <CustomButton url={'https://www.baidu.com'} text="点击打开https网页"/>
         <CustomButton url={'smsto:18352402477'}  text="点击进行发送短信"/> 
         <CustomButton url={'tel:18352402477'} text="点击进行打电话"/>
         <CustomButton url={'mailto:jiangqqlmj@163.com'} text="点击进行发邮件"/>
      </View>
    );
  }
}
const styles = StyleSheet.create({
  button: {
    margin:5,
    backgroundColor: 'white',
    padding: 15,
    borderBottomWidth: StyleSheet.hairlineWidth,
    borderBottomColor: '#cdcdcd',
  },
});
```

# LayoutAnimation 

## 方法

* configureNext(config, onAnimationDidEnd?) 静态方法，进行计算下一个变化的布局动画。其中参数如下:

 - config：该涉及到动画属性有:
    duration   动画持续的时间(毫秒)
    create     创建一个新视图所使用的动画(具体可以查看Anim类型)
    update   当视图被更新的时候所使用的动画(具体可以查看Anim类型)
	delete 
 - onAnimationDidEnd：当动画完成的时候调用的方法，当前仅支持iOS设备

 - onError：当动画发生错误的时候调用的方法,当前仅支持iOS设备

* create(duration,type,createionProp)  静态方法，用来创建configureNext所需的config参数的辅助函数。

## 属性
1. Types:动画类型 (主要涉及:spring,linear,easeInEaseOut,easeIn,easeOut,keyboard)

2. Properties:属性(主要涉及:opacity,scaleXY)

3. configChecker:配置检查器

4. Presets:对象类型(动画效果默认配置项)

5. easeInEaseOut 动画效果

6. linear   动画效果

7. spring   动画效果

** 注意：在Android上需要手动开启动画设置，IOS上默认开启 **

 ```JavaScript
 if (Platform.OS === 'android') {
        UIManager.setLayoutAnimationEnabledExperimental(true)
 }
 ```

## 实例

效果图
![效果图](http://upload-images.jianshu.io/upload_images/5214901-d7acf595e4572767.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
```JavaScript
'use strict';

import React, {Component} from "react";
import {LayoutAnimation, Platform, StyleSheet, Text, TouchableHighlight, View,UIManager} from "react-native";

class CustomButton extends Component {
    render() {
        return (
            <TouchableHighlight
                style={styles.button}
                underlayColor='#a5a5a5'
                onPress={this.props.onPress}>
                <Text >{this.props.text}</Text>
            </TouchableHighlight>);
    }
}

var CustomLayoutAnimation = {
    duration: 800,
    create: {
        type: LayoutAnimation.Types.linear,
        property: LayoutAnimation.Properties.opacity,
    },
    update: {
        type: LayoutAnimation.Types.easeInEaseOut,
    },
    delete:{
        type: LayoutAnimation.Types.linear,
        property: LayoutAnimation.Properties.opacity,
    }
};

export default class FindPage extends Component {
    constructor(props) {
        super(props);
        this.state = {
            views: [],
            num: 0,
        };
        if (Platform.OS === 'android') {
            UIManager.setLayoutAnimationEnabledExperimental(true);
        }
    }

    componentWillUpdate(){

        // LayoutAnimation.spring();
        LayoutAnimation.configureNext(CustomLayoutAnimation);
    }

    _renderAddedView(i) {
        return (
            <View key={i} style={styles.view}>
                <Text style={{color: '#fff'}}>{i}</Text>
            </View>
        );
    }

    _onPressAddView() {
        this.setState({num: Number.parseInt(this.state.num) + 1});
    }

    _onPressRemoveView() {
        this.setState({num: Number.parseInt(this.state.num) - 1});
    }

    render() {
        this.state.views.length = 0;
        for (let i = 0; i < this.state.num; i++) {
            this.state.views.push(this._renderAddedView(i));
        }

        return (
            <View style={{marginTop: 20, margin: 10, flex: 1}}>
                <CustomButton text='添加View' onPress={this._onPressAddView.bind(this)}/>
                <CustomButton text='删除View' onPress={this._onPressRemoveView.bind(this)}/>

                <View style={styles.viewContainer}>
                    {this.state.views}
                </View>

            </View>
        );
    }
}

const styles = StyleSheet.create({
    button: {
        margin: 5,
        backgroundColor: 'white',
        padding: 15,
        borderBottomWidth: StyleSheet.hairlineWidth,
        borderBottomColor: '#cdcdcd'
    },
    view: {
        height: 50,
        width: 50,
        backgroundColor: 'green',
        margin: 8,
        alignItems: 'center',
        justifyContent: 'center'
    },
    viewContainer: {
        flex: 1,
        flexDirection: 'row',
        flexWrap: 'wrap'
    },

});
```

# InteractionManager 交互管理器

该模块和其他相关的调度方法对比:

* requestAnimationFrame():执行控制动画效果的代码
* setImmediate/setTimeout():设置延迟执行任务的时间，该可能会影响到正在执行的动画
* runAfterInteractions():延迟执行任务，该不会影响到正在执行的动画效果

`触摸系统中的单点或者多点触控都是交互动作`，耗时任务会在这些触摸交互动作执行完成之后或者取消以后回调**runAfterInteractions()**方法进行执行。

```JavaScript	
var handle = InteractionManager.createInteractionHandle();
 
//执行动画 (`runAfterInteractions` tasks are queued)
 
//动画执行结束
 
InteractionManager.clearInteractionHandle(handle);
 
//动画清除之后，开始直接runAfterInteractions中的任务
```

runAfterInteractions任务也可以接收一个普通的回调函数或者一个带有gen方法并且返回一个[Promise](http://iyangyao.top/2017/05/03/Promise/)的PromiseTask对象。如果参数是PromiseTask对象，那么任务是异步执行的，也会阻塞。该会等着当前任务执行完毕以后才能执行下一个任务。

默认情况下，队列任务会一次性在setImmediate方法中批量执行。如果你通过setDeadline方法设置一个时间值，那么任务会在延迟该设定值时间进行执行。这时候会调用setTimeout方法进行挂起任务并且阻塞其他任务的执行。这样可以给触摸交互等操作留出时间更好的相应用户操作。

## 方法

1. runAfterInteractions(task)  静态方法,在用户交互和动画结束以后执行任务。
 
2. createInteractionHandle() 静态方法，创建一个句柄(处理器)，通知管理器，某个动画或者交互开始了
3. clearInteractionHandle(handler:Handle)  静态方法，进行清除句柄，通知管理器，某个动画或者交互结束了。
4. setDeadline(deadline:number) 静态方法, 设置延迟时间，该会调用setTimeout方法挂起并且阻塞所有没有完成的任务，然后在eventLoopRunningTime到设定的延迟时间后，然后执行setImmediate方法进行批量执行任务
5. Events:CallExpression
6. addListener:CallExpression

# Timer 定时器

## 方法

* setTimeout,clearTmeout 
 setTimeout(fn, delay) 表示在运行过程中延迟指定的时间后执行，只会执行一次。
* setInterval,clearInterval 
 setInterval(fn, time) 表示在运行过程中每隔指定的时间进行调用方法，该调用方法会执行多次。
* setImmediate,clearImmediate 
* requestAnimationFrame,cancelAnimationFrame
 requestAnimationFrame() 每帧刷新时候调用 
 
## TimerMinxin

很多导致React Native应用频繁崩溃的原因是因为组件被卸载，但是定时器还处于被激活的状态。使用TimerMinxin模块解决这个问题。
如：
setTimeout(fn, 500) 替换为 this.setTimeout(fn, 500)
这样组件被卸载时，所有有关的定时器相关的事件被自动清除。

** 安装 TimerMinxin **
```sh
npm install react-native-timer-mixin --save
```

`注意：以上只适合ES5使用，若ES6开发，无法直接使用TimerMixin，在ES6语法中实现，如下`

```JavaScript
import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View,
} from 'react-native';
class TimersDemo extends Component {
  constructor(props) {
      super(props);  
      this.state={
        content:'',
      }
  }
  componentDidMount() {
    this.timer = setTimeout(
      () => {
        this.setState({content:'我是定时器打印的内容...One'})
      },
      500
    );
    this.timer_two = setTimeout(
      () => {
        this.setState({msg:'我是定时器打印的内容...Two'})
      },
      1000
    );
  }
  componentWillUnmount() {
    this.timer && clearTimeout(this.timer);
    this.timer_two && clearTimeout(this.timer_two);
  }
  render() {
    return (
      <View style={{margin:20}}>
        <Text style={styles.welcome}>
           定时器实例
        </Text>
        <Text>{this.state.content}</Text>
        <Text>{this.state.msg}</Text>
      </View>
    );
  }
}
const styles = StyleSheet.create({
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
  },
});
 
AppRegistry.registerComponent('TimersDemo', () => TimersDemo);	
```

**以上是setTimeout 方法，setInterval等也类似。**

# Share 分享

## 方法

* share(Content, Options)

## 属性 
1. Content(内容)-Android,iOS通用
 - message   需要进行分享的信息
 - title    分享的标题
 - url(地址)-iOS适配
 **注意：分享的地址，url和message至少需要设置其中一个**

2. Options（可选参数）
 - excludedActivityTypes   这个适配iOS版本
 - tintColor 这个适配iOS版本
 - dialogTitle    这个适配Android版本

## 实例

```JavaScript
import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View,
  Share,
  TouchableHighlight
} from 'react-native';
 
class ShareDemo extends Component {
  constructor(props) {
    super(props);
    this._shareMessage = this._shareMessage.bind(this);
    this._shareText = this._shareText.bind(this);
    this._showResult = this._showResult.bind(this);
    this.state = {
      result: ''
    };
  }
 
  render() {
    return (
      <View>
        <TouchableHighlight style={styles.wrapper}
          onPress={this._shareMessage}>
          <View style={styles.button}>
            <Text>点击分享文本</Text>
          </View>
        </TouchableHighlight>
        <TouchableHighlight style={styles.wrapper}
          onPress={this._shareText}>
          <View style={styles.button}>
            <Text>点击分享文本,URL和标题</Text>
          </View>
        </TouchableHighlight>
        <Text>{this.state.result}</Text>
      </View>
    );
  }
 
  _shareMessage() {
    Share.share({
      message: '我是被分享的本文信息'
    })
    .then(this._showResult)
    .catch((error) => this.setState({result: 'error: ' + error.message}));
  }
 
  _shareText() {
    Share.share({
      message: '我是被分享的本文信息',
      url: 'http://www.lcode.org',
      title: 'React Native'
    }, {
      dialogTitle: '分享博客地址',
      excludedActivityTypes: [
        'com.apple.UIKit.activity.PostToTwitter'
      ],
      tintColor: 'green'
    })
    .then(this._showResult)
    .catch((error) => this.setState({result: 'error: ' + error.message}));
  }
 
  _showResult(result) {
    if (result.action === Share.sharedAction) {
      if (result.activityType) {
        this.setState({result: 'shared with an activityType: ' + result.activityType});
      } else {
        this.setState({result: 'shared'});
      }
    } else if (result.action === Share.dismissedAction) {
      this.setState({result: 'dismissed'});
    }
  }
}
 
const styles = StyleSheet.create({
  wrapper: {
    borderRadius: 5,
    marginBottom: 5,
  },
  button: {
    backgroundColor: '#eeeeee',
    padding: 10,
  },
});
 
AppRegistry.registerComponent('ShareDemo', () => ShareDemo);
```

# PermissionsAndroid 权限检测和请求

## 属性和方法

* checkPermission(permission)  该方法返回一个Promise对象，其中resolve值返回一个boolean值用来表示权限是否已经被授权。

* requestPermission(permission,rationale?)   该方法返回一个Promise对象，其中resolve值返回一个boolean值，用来表示权限授权请求是否允许还是拒绝。第二个rationale参数代表请求授权的弹框确认。

## 实例

```JavaScript
import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View,
  PermissionsAndroid,
  TextInput,
  TouchableWithoutFeedback
} from 'react-native';
 
class PermissionDemo extends Component {
  constructor(props, context) {
    super(props, context);
    this.state = {
       permission: PermissionsAndroid.PERMISSIONS.WRITE_EXTERNAL_STORAGE,
       hasPermission: 'Not Checked',
      };
  }
  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.text}>Permission Name:</Text>
        <TextInput
          autoFocus={true}
          autoCorrect={false}
          style={styles.singleLine}
          onChange={this._updateText}
          defaultValue={this.state.permission}
        />
        <TouchableWithoutFeedback onPress={this._checkPermission}>
          <View>
            <Text style={[styles.touchable, styles.text]}>Check Permission</Text>
          </View>
        </TouchableWithoutFeedback>
        <Text style={styles.text}>Permission Status: {this.state.hasPermission}</Text>
        <TouchableWithoutFeedback onPress={this._requestPermission}>
          <View>
            <Text style={[styles.touchable, styles.text]}>Request Permission</Text>
          </View>
        </TouchableWithoutFeedback>
      </View>
    );
  }
 
  _updateText = (event: Object) => {
    this.setState({
      permission: event.nativeEvent.text,
    });
  }
 
  _checkPermission = async () => {
    let result = await PermissionsAndroid.checkPermission(this.state.permission);
    this.setState({
      hasPermission: (result ? '授权成功' : '授权失败') + ' for ' +
        this.state.permission,
    });
  }
 
  _requestPermission = async () => {
    let result = await PermissionsAndroid.requestPermission(
      this.state.permission,
      {
        title: '权限请求',
        message:
          '该应用需要如下权限 ' + this.state.permission +
          ' 请授权!'
      },
    );
    this.setState({
      hasPermission: (result ? '授权成功' : '授权失败') + ' for ' +
        this.state.permission,
    });
  }
}
 
const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: 'white',
  },
  singleLine: {
    fontSize: 16,
    padding: 4,
  },
  text: {
    margin: 10,
  },
  touchable: {
    color: '#007AFF',
  },
});
 
AppRegistry.registerComponent('PermissionDemo', () => PermissionDemo);
```

## 官方危险权限

```
READ_CALENDAR: 'android.permission.READ_CALENDAR',
WRITE_CALENDAR: 'android.permission.WRITE_CALENDAR',
CAMERA: 'android.permission.CAMERA',
READ_CONTACTS: 'android.permission.READ_CONTACTS',
WRITE_CONTACTS: 'android.permission.WRITE_CONTACTS',
GET_ACCOUNTS:  'android.permission.GET_ACCOUNTS',
ACCESS_FINE_LOCATION: 'android.permission.ACCESS_FINE_LOCATION',
ACCESS_COARSE_LOCATION: 'android.permission.ACCESS_COARSE_LOCATION',
RECORD_AUDIO: 'android.permission.RECORD_AUDIO',
READ_PHONE_STATE: 'android.permission.READ_PHONE_STATE',
CALL_PHONE: 'android.permission.CALL_PHONE',
READ_CALL_LOG: 'android.permission.READ_CALL_LOG',
WRITE_CALL_LOG: 'android.permission.WRITE_CALL_LOG',
ADD_VOICEMAIL: 'com.android.voicemail.permission.ADD_VOICEMAIL',
USE_SIP: 'android.permission.USE_SIP',
PROCESS_OUTGOING_CALLS: 'android.permission.PROCESS_OUTGOING_CALLS',
BODY_SENSORS:  'android.permission.BODY_SENSORS',
SEND_SMS: 'android.permission.SEND_SMS',
RECEIVE_SMS: 'android.permission.RECEIVE_SMS',
READ_SMS: 'android.permission.READ_SMS',
RECEIVE_WAP_PUSH: 'android.permission.RECEIVE_WAP_PUSH',
RECEIVE_MMS: 'android.permission.RECEIVE_MMS',
READ_EXTERNAL_STORAGE: 'android.permission.READ_EXTERNAL_STORAGE',
WRITE_EXTERNAL_STORAGE: 'android.permission.WRITE_EXTERNAL_STORAGE',
```




