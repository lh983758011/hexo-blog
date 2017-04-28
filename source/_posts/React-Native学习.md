---
title: React Native学习
date: 2017-04-25 14:53:30
tags:
- React Native
- 移动开发
categories: [React Native]
---
React Native基础学习之路

<!-- more --> 

# 学习网站

1. [江清清React Native专题](http://www.lcode.org/react-native/)
2. [仿京东](http://blog.csdn.net/yuanguozhengjust/article/details/50538651)

# 组件学习

## 基本组件

### Image

#### 加载图片资源方式

1. 加载本地项目路径的图片资源（从0.14版本开始支持）
```
<Image source={require('./img/my_icon.png')} />
```
**注意：require 必须是静态的字符串信息，不能拼接。**

2. 加载原生图片资源如Android的drawable和IOS的asset下面的图片资源
```JavaScript
<Image source={{uri: 'ic_launcher'}} />
```

3. 加载网络图片
```
<Image source={{uri: 'http://.....'}} />
```

#### 属性

* onLayout   (function) 当Image布局发生改变的，会进行调用该方法，调用的代码为:
```
{nativeEvent: {layout: {x, y, width, height}}}.
```
* onLoad (function):当图片加载成功之后，回调该方法

* onLoadEnd (function):当图片加载失败回调该方法，该不会管图片加载成功还是失败

* onLoadStart (fcuntion):当图片开始加载的时候调用该方法

* resizeMode  缩放比例,可选参数('cover', 'contain', 'stretch') 该当图片的尺寸超过布局的尺寸的时候，会根据设置Mode进行缩放或者裁剪图片

* source {uri:string} 进行标记图片的引用，该参数可以为一个网络url地址或者一个本地的路径

#### Image样式风格

1. FlexBox  支持弹性盒子风格
2. Transforms  支持属性动画                
3. resizeMode  设置缩放模式
4. backgroundColor 背景颜色
5. borderColor     边框颜色 
6. borderWidth 边框宽度
7. borderRadius  边框圆角
8. overflow 设置图片尺寸超过容器可以设置显示或者隐藏('visible','hidden')
9. tintColor  颜色设置         
10. opacity 设置不透明度0.0(透明)-1.0(完全不透明)

### TextInput

#### 实例

```
<TextInput
    style={{height: 40, borderColor: 'gray', borderWidth: 1}}
    onChangeText={(text) => this.setState({text})}
    value={this.state.text}
  />
```

#### 属性

1. View 支持View的相关属性
2. autoCapitalize  控制TextInput输入的字符进行切换成大写(可选择参数:'none', 'sentences', 'words', 'characters')
> none:不自动切换任何字符成大写
> sentences:默认每个句子的首字母变成大写
> words:每个单词的首字母变成大写
> characters:每个字母全部变成大写
3. autoCorrect  bool  设置拼写自动修正功能 默认为开启(true)
4. autoFocus bool  设置是否默认获取到焦点默认为关闭(false)。该需要componentDidMount方法被调用之后才会获取焦点哦(componentDidMount是React组件被渲染之后React主动回调的方法)
5. defaultValue  string 给文本输入设置一个默认初始值。
6. editable bool  设置文本框是否可以编辑 默认值为true,可以进行编辑
7. keyboardType  键盘类型(可选参数:"default", 'email-address', 'numeric', 'phone-pad', "ascii-capable", 'numbers-and-punctuation', 'url', 'number-pad', 'name-phone-pad', 'decimal-pad', 'twitter', 'web-search') 该用来选择默认弹出键盘的类型例如我们甚至numeric就是弹出数字键盘。鉴于平台的原因如下的值是所有平台都可以进行通用的
> default
> numeric	数字键盘
> email-address  邮箱地址
8. maxLength  number  可以限制文本输入框最大的输入字符长度
9. multiline bool  设置可以输入多行文字，默认为false(表示无论文本输入多少，都是单行显示)
10. onBlur  function 监听方法，文本框失去焦点回调方法
11. onChange function 监听方法,文本框内容发生改变回调方法
12. onChangeText  function监听方法，文本框内容发生改变回调方法，该方法会进行传递文本内容
13. onEndEditing  function监听方法，当文本结束文本输入回调方法
14. onFocus  function 监听方法  文本框获取到焦点回调方法
15. onLayout  function监听方法  组价布局发生变化的时候调用，调用方法参数为 {x,y,width,height}
16. onSubmitEditing function监听方法，当编辑提交的时候回调方法。不过如果multiline={true}的时候，该属性就不生效
17. placeholder string 当文本输入框还没有任何输入的时候，默认显示信息，当有输入的时候该值会被清除
18. placeholderText Color  string 设置默认信息颜色(placeholder)
19. secureTextEntry  bool 设置是否为密码安全输入框 ，默认为false
20. style 风格属性  可以参考Text组件风格
21. value  string 输入框中的内容值
** 以上是一些Android，iOS平台通用的属性，下面根据官网的文档，我这边组要讲解一下适用于Android平台的属性方法 ** 
22. numberOfLines number设置文本输入框行数，该需要首先设置multiline为true,设置TextInput为多行文本。
23. textAlign 设置文本横向布局方式 可选参数('start', 'center', 'end')
24. textAlignVertical 设置文本垂直方向布局方式 可选参数('top', 'center', 'bottom')
25. underlineColorAndroid  设置文本输入框下划线的颜色 
> 设置下划线为透明
```
underlineColorAndroid={'transparent'} 
```

### ProgressBarAndroid

#### 实例
```JavaScript
'use strict';
import React, {
  AppRegistry,
  Component,
  StyleSheet,
  Text,
  View,
  ProgressBarAndroid,
} from 'react-native';
 
class ProgressBarDemo extends Component {
  render() {
    return (
      <View >
        <Text>
            ProgressBarAndroid控件实例
        </Text>
        <ProgressBarAndroid  color="red" styleAttr='Inverse'/>
        <ProgressBarAndroid  color="green" styleAttr='Horizontal' progress={0.2} 
            indeterminate={false} style={{marginTop:10}}/>
        <ProgressBarAndroid  color="green" styleAttr='Horizontal'
            indeterminate={true} style={{marginTop:10}}/>
        <ProgressBarAndroid  color="black" styleAttr='SmallInverse'
            style={{marginTop:10}}/>
        <ProgressBarAndroid  styleAttr='LargeInverse'
            style={{marginTop:10}}/>
      </View>
    );
  }
}
AppRegistry.registerComponent('ProgressBarDemo', () => ProgressBarDemo);
```

#### 属性

1. 支持View控件的属性方法 (这些属性是从View控件中继承下来)  例如:大小,布局,边距啊

2. color  设置进度的颜色属性值

3. indeterminate 设置是否要显示一个默认的进度信息，该如果styleAttr的风格设置成Horizontal的时候该值必须设置成false

4. progress  number  设置当前的加载进度值(该值在0-1之间)

5. styleAttr    进度条框的风格 ，可以取的值如下:
* Horizontal
* Small
* Large
* Inverse
* SmallInverse
* LargeInverse

### DrawerLayoutAndroid 侧边栏

#### 实例

```
//侧边栏内容菜单
 _renderNavigationView() {
        return (
            <View style={{flex: 1}}>
                <Text>Items1</Text>
                <Text>Items2</Text>
            </View>);
    }

//侧边栏布局
<DrawerLayoutAndroid
                drawerWidth={260}
                drawerPosition={DrawerLayoutAndroid.positions.left}
                renderNavigationView={this._renderNavigationView}
            >
                <View style={{flex: 1}}>
                    <Header />
                    <TabNavigator hidesTabTouch={true} tabBarStyle={styles.tab}>
                        {this._renderTabItem(HOME_NORMAL, HOME_FOCUS, HOME, <HomePage />)}
                        {this._renderTabItem(CATEGORY_NORMAL, CATEGORY_FOCUS, CATEGORY, MainScreen._createChildView(CATEGORY))}
                        {this._renderTabItem(FAXIAN_NORMAL, FAXIAN_FOCUS, FAXIAN, MainScreen._createChildView(FAXIAN))}
                        {this._renderTabItem(CART_NORMAL, CART_FOCUS, CART, MainScreen._createChildView(CART))}
                        {this._renderTabItem(PERSONAL_NORMAL, PERSONAL_FOCUS, PERSONAL, MainScreen._createChildView(PERSONAL))}
                    </TabNavigator>
                </View>
            </DrawerLayoutAndroid>
```

#### 属性

1. View的属性使用  继承了View控件的属性信息(例如:宽和高,背景颜色,边距等相关属性样式)

2. drawerPosition   参数为枚举类型(DrawerConsts.DrawerPosition.Left, DrawerConsts.DrawerPosition.Right)

 - 进行指定导航菜单用那一侧进行滑动出来，根据官方实例最终传入的两个枚举值分别为:DrawerLayoutAndroid.positions.Left和DrawerLayoutAndroid.positions.Right

3. drawerWidth  进行指定导航菜单视图的宽度，也就是说该侧面导航视图可以从屏幕边缘拖拽到屏幕的宽度距离

4. keyboardDismissMode    参数为枚举类型('none','on-drag') 进行指定在导航视图拖拽的过程中是否要隐藏键盘

 - none   (默认值),默认不会隐藏键盘
 - on-drag  当拖拽开始的时候进行隐藏键盘

5. onDrawerClose   function 方法，当导航视图被关闭后进行回调该方法

6. onDrawerOpen   function 方法，当导航视图被打开后进行回调该方法

7. onDrawerSlide  function  方法，当导航视图和用户进行交互的时候调用该方法

8. onDrawerStateChanged function方法，该当导航视图的状态发生变化的时候调用该方法。该状态会有以下三种状态

 - idle (空闲) 表示导航视图上面没有任何交互状态
 - dragging (正在拖拽中)   表示用户正在和导航视图产生交互动作
 - settling (暂停-刚刚结束)  表示用户 刚刚结束和导航视图的交互动作，当前导航视图正在打开或者关闭拖拽滑动动画效果

9. renderNavigationView  function 方法，该方法进行渲染一个导航抽屉的视图(用于用户从屏幕边缘拖拽出来)

### ScrollView

#### 属性

1. View相关属性样式全部继承(例如:宽和高,背景颜色,边距等相关属性样式)

2. contentContainerStyle  样式风格属性(传入StyleSheet创建的Style文件)。该样式会作用于被ScrollView包裹的所有的子视图。实例如下:
```
return ( 
      <ScrollView contentContainerStyle={styles.contentContainer}> </ScrollView> 
      ); 
   …
 var styles = StyleSheet.create({ 
          contentContainer: { 
                   paddingVertical: 20 
               } 
         });
```
3. horizontal   表示ScrollView是横向滑动还是纵向滑动。该默认为false表示纵向滑动

4. keyboardDismissMode   枚举类型表示键盘隐藏类型，可选值('none', "interactive", 'on-drag')  三个值的意义分别如下:
* none  默认值，表示在进行拖拽滑动的时候不隐藏键盘
* on-drag   表示在进行拖拽滑动开始的时候隐藏键盘
* interactive  表示当拖拽触摸移动的同时隐藏键盘，向上拖拽的时候取消隐藏。不过在Android平台上面该选项不支持，所以会和'none'一样的效果。

5. keyboardShouldPersistTaps  该属性默认为false，表示如果当前是textinput控件，并且键盘是弹出状态的话，点击textinput之外地方，会进行隐藏键盘。反之不会有效果，键盘还是成打开状态。

6. onContentSizeChange  function  该当滚动视图的内容尺寸大小发生变化的时候进行调用

7. onScroll  function  该方法在滚动的时候每frame(帧)调用一次。该方法事件调用的频率可以使用scrollEventThrottle属性进行设置。

8. refreshControl   element 设置元素控件，该可以进行指定RefreshControl组件。这样可以为ScrollView添加下拉刷新的功能.

9. removeClippedSubviews  测试属性 当该值为true的时候。在ScrollView视图之外的视图(该视图的overflow属性值必须要为hidden)会从被暂时移除，该设置可以提高滚动的性能。

10. showsHorizontalScrollIndicator   该值设置是否需要显示横向滚动指示条

11. showsVerticalScrollIndicator 该值设置是否需要显示纵向滚动指示条

12. sendMomentumEvents   当ScrollView有onMomentumScrollBegin或者onMomentumScrollEnd方法设置，该sendMomentumEvents值设置为true的时候。变化的事件信息会通过该Android框架自动发送出来，然后之前设置的方法进行捕捉。

### ToolbarAndroid

#### 实例
```JavaScript
render: function() {
  return (
    <ToolbarAndroid
	  style={styles.toolbar}
      logo={require('./app_logo.png')}
      title="AwesomeApp"
      actions={[{title: 'Settings', icon: require('./icon_settings.png'), show: 'always'}]}
      onActionSelected={this.onActionSelected} />
  )
},
onActionSelected: function(position) {
  if (position === 0) { // index of 'Settings'
    showSettings();
  }
}
```

#### 属性

1. View相关属性样式全部继承(例如:宽和高,背景颜色,边距等相关属性样式)

2. actions 设置功能列表信息属性 传入的参数信息为: [{title: string, icon: optionalImageSource, show: enum('always', 'ifRoom', 'never'), showWithText: bool}]   进行设置功能菜单中的可用的相关功能。该会在显示在组件的右侧(显示方式为图标或者文字)，如果界面上面区域已经放不下了，该会加入到隐藏的菜单中(弹出进行显示)。该属性的值是一组对象数组，每一个对象包括以下以下一些参数:

 - title: 必须的，该功能的标题
 - icon: 功能的图标  采用该代码进行获取 require('./some_icon.png')
 - show: 该设置图标直接显示，还是隐藏或者显示在弹出菜单中。always代表总是显示,ifRoom代表如果Bar中控件够进行显示，或者never代表使用直接不显示
 - showWithText：boolean 进行设置图标旁边是否要显示标题信息
 
3. contentInSetEnd  number 该用于设置ToolBar的右边和屏幕的右边缘的间距。

4. contentInsetStart number 该用于设置ToolBar的左边和屏幕的左边缘的间距。

5. logo  optionalImageSource  可选图片资源  用于设置Toolbar的Logo图标

6. navIcon optionalImageSource 可选图片资源 用于设置导航图标

7. onActionSelected function方法 当我们的功能被选中的时候回调方法。该方法只会传入唯一一个参数:点击功能在功能列表中的索引信息

8. onIconClicked function 当图标被选中的时候回调方法

9. overflowIcon  optionalImageSource 可选图片资源 设置功能列表中弹出菜单中的图标

10. rtl   设置toolbar中的功能顺序是从右到左(RTL:Right To Left)。为了让该效果生效，你必须在Android应用中的AndroidMainifest.xml中的application节点中添加android:supportsRtl="true"，然后在你的主Activity(例如:MainActivity)的onCreate方法中调用如下代码:setLayoutDirection(LayoutDirection.RTL)。

11. subtitle  string 设置toolbar的副标题

12. subtitleColor  color  设置设置toolbar的副标题颜色

13. title string  设置toolbar标题

14. titleColor color 设置toolbar的标题颜色

### Piker 选择器

#### 实例

```
<Picker 
          style={{width:200}}
          selectedValue={this.state.language}
          onValueChange={(value) => this.setState({language: value})}>
          <Picker.Item label="Java" value="java" />
          <Picker.Item label="JavaScript" value="javaScript" />
        </Picker>
```

#### 属性

1. View相关属性样式全部继承(例如:宽和高,背景颜色,边距等相关属性样式)
2. onValueChange  function方法,当选择器item被选择的时候进行调用。该方法被调用的时候回传入一下两个参数
 - itemValue:该属性值为被选中的item的属性值

 - itemPosition:该选择器被选中的item的索引position

3. selectedValue: any任何参数值，选择器选中的item所对应的值，该可以是一个字符串或者一个数字

4. style pickerStyleType 该传入style样式，设置picker的样式风格

5. enabled bool 如果该值为false，picker就无法被点击选中。例如:用户无法进行做出选择

6. mode enum ('dialog','dropdown')  选择器模式。在Android平台上面，设置mode可以控制用户点击picker弹出的样式风格
 - 'dialog': 该值为默认值，进行弹出一个模态dialog(弹出框)

 - 'dropdown':以picker视图为基础，在该视图下面弹出下拉框

7. prompt string  设置picker的提示语(标题),在Android平台上面，模式设置成'dialog',显示弹出框的标题

### Touchable系列组件

包括：触摸、点击、长按、反馈

#### TouchableWithoutFeedback

 - 基本不使用，因为没有想原生那样的反馈表现，而更像Web
 
#### TouchableHighlight 触摸点击高亮效果

 - 该组件封装了触摸事件，点击时透明度降低，同时响应的颜色变暗或变亮
 
 - 属性
  - activeOpacity  number 该用来设置视图在进行触摸的时候，要要显示的不透明度(通常在0-1之间)
  - onHideUnderlay  function  方法 当底层被隐藏的时候调用
  - onShowUnderlay  function 方法 当底层显示的时候调用
  - style  可以设置控件的风格演示，该风格演示可以参考View组件的style
  - underlayColor  当触摸或者点击控件的时候显示出的颜色
 - 实例
 ```
  <TouchableHighlight 
          underlayColor="rgb(210, 230, 255)"
          activeOpacity={0.5}  
          style={{ borderRadius: 8,padding: 6,marginTop:5}}
          >
             <Text style={{fontSize:16}}>点击我</Text>
        </TouchableHighlight>
 ```
 
 ** 【特别声明】TouchableHighlight只支持一个子节点，如果你需要设置多个子视图组件，那么就需要使用View节点进行包装。 **

#### TouchableOpacity 透明变化

 - 该组件封装了触摸事件，当点击时，透明度降低。

 - 属性
  - activeOpacity  number  设置当用户触摸的时候，组件的透明度
 - 实例
 ```
 <TouchableOpacity style={{marginTop:20}}>
           <Text style={{fontSize:16}}>点击改变透明度</Text>
        </TouchableOpacity>
 ```
 
#### TouchableNativeFeedback 

 - 该封装了可以进行响应触摸事件的组件(仅限Android平台)。在Android平台上面该该组件可以使用原生平台的状态资源来显示触摸状态变化。该组件触摸反馈的背景图资源可以通过background属性进行自定义设置
 **【特别注意】现如今该组件只是支持仅有一个View的子视图实例(作为子节点)。在底层实现上面实际上面是创建一个新的RCTView的节点来进行替换当前的View节点视图，并且可以携带一些附加的属性。**

 - 属性
  - background  backgroundPropType  该用来设置背景资源的类型，该属性会传入一个type属性和依赖的额外资源的data的对象。官方推荐采用如下的静态方法来进行生成该dictionary对象
  
   > 1.TouchableNativeFeedback.SelectableBackground()   该会创建使用android默认主题背景(?android:attr/selectableItemBackground)

   > 2.TouchableNativeFeedback.SelectableBackgroundBorderless()  该会创建使用android默认的无框的主题背景(?android:attr/selectableItemBackgroundBorderless)。不过该参数需要Android API 21+才可以使用

   > 3.TouchableNativeFeedback.Ripple(color,borderless)  该会创建当组件被按下的时候有一个水滴的效果.通过color参数指定颜色，如果borderless为true的时候，那么该水滴效果会渲染到该组件视图的外边。同样该背景类型参数也需要Android API 21+才可以使用
  
 - 实例
 ```
 <TouchableNativeFeedback style={{marginTop:20}} 
            background={TouchableNativeFeedback.SelectableBackground()}>
            <View style={{width: 150, height: 100,}}>
               <Text style={{margin: 30}}>Button</Text>
            </View>  
        </TouchableNativeFeedback>
 ```
 
## 开源组件

1. [TabNavigator 导航页](https://github.com/exponentjs/react-native-tab-navigator)
国外大神在TabBar之上封装
引入库
```
npm install i react-native-tab-navigator --save
```

使用
```JavaScript
import TabNavigator from 'react-native-tab-navigator';  

export default class MainScreen extends Component {  
    render() {  
        return (  
            <View style={{flex:1}}>  
                <Header />  
                <TabNavigator>  
                      
                </TabNavigator>  
            </View>  
        );  
    }  
} 
```
TabNavigator的样式设置
属性：
> * sceneStyle: 场景样式，即Tab页容器的样式
> * tabBarStyle: TabBar的样式
> * tabBarShadowStyle：TabBar阴影的样式，不过对于扁平化的设计，这个属性应该用处不大
> * hidesTabTouch：bool类型，即是否隐藏Tab按钮的按下效果

Item的构建
犹如Android中的Fragment，IOS中的UIView，TabNavigator以`<TabNavigator.Item>`呈现
属性：
> * renderIcon: 必填项，即图标，但为function类型，所以这里需要用到Arrow Function
> * renderSelectedIcon: 选中状态的图标，非必填，也是function类型
> * badgeText: 即Tab右上角的提示文字，可为String或Number，类似QQ中Tab右上角的消息提示，非必填
> * renderBadge: 提示角标渲染方式，function类型，类似render的使用，非必填
> * title: 标题，String类型，非必填
> * titleStyle: 标题样式，style类型，非必填
> * selectedTitleStyle: 选中标题样式，style类型，非必填
> * selected: bool型，是否选中状态，可使用setState进行控制，默认false
> * onPress: function型，即点击事件的回调函数，这里需要控制的是state，而切换页面已经由控件本身帮我们实现好了
> * allowFontScaling: bool型，是否允许字体缩放，默认false

**注意：页面切换，在TabNavigator.Item中，可将其置于`<TabNavigator.Item>{<View/>}</TabNavigator.Item>`之中，即作为Item的子元素存在**
```JavaScript
_renderTabItem(img, selectedImg, tag, childView) {  
    return (  
        <TabNavigator.Item  
            selected={this.state.selectedTab === tag}  
            renderIcon={() => <Image style={styles.tabIcon} source={img}/>}  
            renderSelectedIcon={() => <Image style={styles.tabIcon} source={selectedImg}/>}  
            onPress={() => this.setState({ selectedTab: tag })}>  
            {childView}  
        </TabNavigator.Item>  
    );  
}  
```

2. [viewpager 轮播页](https://github.com/race604/react-native-viewpager)

引入库
```
npm install i react-native-viewpager --save
```

使用
```JavaScript
<ViewPager  
    style={{height:130}}  
    dataSource={this.state.dataSource}  
    renderPage={this._renderPage}  
    isLoop={true}  
    autoPlay={true}/>  
```

属性：
> * style：样式，和其他控件设置方式类似
> * dataSource：即在构造函数中利用dataSource对象和图片数组常量，创建的真实dataSource
> * renderPage：方法类型，返回一段JSX，用于指定ViewPager每页的内容，该方法写法如下，`切忌根据WebStorm的提示加上static！`：
```JavaScript

constructor(props) {  
    super(props);  
    // 用于构建DataSource对象  
    var dataSource = new ViewPager.DataSource({  
        pageHasChanged: (p1, p2) => p1 !== p2,  
    });  
    // 实际的DataSources存放在state中  
    this.state = {  
        dataSource: dataSource.cloneWithPages(BANNER_IMGS)  
    }  
}  

_renderPage(data, pageID) {  
    return (  
        <Image  
            source={data}  
            style={styles.page}/>  
    );  
}
```  
> * isLoop：是否循环播放，按照示例代码设置即可
> * autoPlay：是否自动播放，按照示例代码设置即可
> * locked: 设置为true即禁用ViewPager的点击
> * onChangePage: 页面切换的回调函数
> * renderPageIndicator: 自定义指示器样式的渲染
> * animation：如果觉得原始的效果不满意，可以在这个字段中设置一个函数，自定义动画效果

**注意：有个bug，就是在ViewPager源码中，Indicator的位置不对，及ViewPager的高度不对，如果单在外面设置style高度的话没效果，需要在源码修改 render 方法的 return 的 View的 style 为 style={this.props.style}，外面的设置ViewPager的高度才会有效**

# 开发工具

## WebStorm

### 插件



#### 代码提示

插件地址：<https://github.com/virtoolswebplayer/ReactNative-LiveTemplate>

下载方法:

- 首先大家可以把该项目下载或者如下命令clone下来

 - git clone https://github.com/virtoolswebplayer/ReactNative-LiveTemplate


- 安装方法一(Windows和Mac都支持):

 - file -> import settings -> ReactNative.jar


- 安装方法二(该方法只是针对Mac):

 - 将ReactNative.xml复制到 ~/Library/Preferences/WebStorm11/templates 重启 WebStrom


使用方法:

- 通用方法
直接输入组件 或 Api 名称的首字母, 比如想要 View ,只要输入 V自动提示代码里就会看到 View
- StyleSheet属性提示
首先 按下 command + J , 然后输入属性名的 首字母

提示内容包括：
* 组件名称
* Api 名称
* 所有StyleSheets属性
* 调用ReactNative组件时, 首先 按下 command + J , 然后输入属性名的 首字母 如输入onP 自动提示 onPress, onPressIn, onPressOut, ....

## Atom

# 调试

http://localhost:8081/debugger-ui