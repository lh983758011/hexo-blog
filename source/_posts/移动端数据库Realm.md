---
title: 移动端数据库Realm
date: 2017-05-05 14:36:41
tags:
- 数据库
- React Native 
- Android
categories: [Realm]
---
移动端数据库Realm

<!-- more -->

# 构建工程

* Realm官网地址：https://realm.io/ 

* Realm 安装
 ```sh
 npm install realm --save
 ```

* 链接你的项目到Realm（link your project to the realm native module.）

 ```
 React Native >= 0.31.0
 
 react-native link realm
 
 React Native < 0.31.0
 
 rnpm link realm
 ```
* 链接结果
 1. Add the following lines to android/settings.gradle:
 ```
 gradle include ':realm' project(':realm').projectDir = new File(rootProject.projectDir, '../node_modules/realm/android')
 ```
 
 2. Add the compile line to the dependencies in android/app/build.gradle:
 ```
 gradle dependencies { compile project(':realm') }
 ```
 
 3. Add the import and link the package in MainApplication.java:
 
 ```Java
 import io.realm.react.RealmReactPackage; // add this import
 
 public class MainApplication extends Application implements ReactApplication {
     @Override
     protected List<ReactPackage> getPackages() {
         return Arrays.<ReactPackage>asList(
             new MainReactPackage(),
             new RealmReactPackage() // add this line
         );
     }
 }
 ```

* React Native
 ```JavaScript
 import Realm from 'realm;
 
 class <project-name> extends Component {
  render() {
    let realm = new Realm({
      schema: [{name: 'Dog', properties: {name: 'string'}}]
    });
 
    realm.write(() => {
      realm.create('Dog', {name: 'Rex'});
    });
 
    return (
      <View style={styles.container}>
        <Text style={styles.welcome}>
          Count of Dogs in Realm: {realm.objects('Dog').length}
        </Text>
      </View>
    );
  }
 }
 You can then run your app on a device and in a simulator!
 
 Install exa
 ```

