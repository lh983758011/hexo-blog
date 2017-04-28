---
title: React Native 环境配置
date: 2017-04-27 16:56:49
tags:
- React Native
categories: [React Native]
---

环境配置

<!-- more --> 

# 环境配置

1. 安装Node.js环境 <https://nodejs.org/en/>  
node -v  测试安装是否成功
2. 克隆RN环境
```
git clone https://github.com/facebook/react-native.git 
```

3. 进入刚刚目录下的**react-native**目录下的**react-native-cli**目录，输入
```
npm install -g
```
安装好之后，可以命令行下就有react-native命令了

4. 创建RN工程，react-native init AwesomeProject（切换到react-native目录下）
5. 运行RN工程，进入工程目录，输入react-native start
6.在浏览器中访问 http://localhost:8081/index.android.bundle?platform=android 测试服务器是否启动成功。
7.进入项目目录，输入react-native run-android

**注意：在此可能会遇到 unsupport version 52**

只能通过手动打包方式，将js资源以Bundle形式放入安装包中
在项目目录下输入以下命令
```
react-native bundle --platform android --dev false --entry-file index.android.js（所要打包的js） \ --bundle-output android/app/assets/FirstProject.android.bundle（bundle输出路径，一般放到asset里面） \ --assets-dest android/app/res
```

# 应用到Android程序中
在原生安卓工程中移植React Native
1. 创建android工程

2. 添加  依赖 
```Java
compile 'com.facebook.react:react-native:+'
```

 在根目录的build.gradle 中
```Java
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.2.3'

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {
        mavenLocal()
        jcenter()
        maven {
            // All of React Native (JS, Obj-C sources, Android binaries) is installed from npm
            url "$rootDir/../node_modules/react-native/android"
        }
    }
}
```

3. 原生Activity继承ReactActivity

 ```Java
 public class MainActivity extends ReactActivity {
  /**
   * Returns the name of the main component registered from JavaScript.
   * This is used to schedule rendering of the component.
   */
  @Override
  protected String getMainComponentName() {
    return "FirstProject";
  }

 }
 ```

4. Application实现ReactApplication
 ```Java
	public class MainApplication extends Application implements ReactApplication {

	  private final ReactNativeHost mReactNativeHost = new ReactNativeHost(this) {
		@Override
		public boolean getUseDeveloperSupport() {
		  return BuildConfig.DEBUG;
		}

		@Override
		protected List<ReactPackage> getPackages() {
		  return Arrays.<ReactPackage>asList(
			  new MainReactPackage()
		  );
		}
	  };

	  @Override
	  public ReactNativeHost getReactNativeHost() {
		return mReactNativeHost;
	  }

	  @Override
	  public void onCreate() {
		super.onCreate();
		SoLoader.init(this, /* native exopackage */ false);
	  }
	}
 ```

5. 在目录下添加package.json  用命令：npm init
并在package.json 中 配置

6. 在目录下安装依赖模块 node_modules 用命令：npm install

7. 启动node服务，用命令：npm start