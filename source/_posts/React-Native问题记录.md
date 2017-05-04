---
title: React Native问题记录
date: 2017-05-02 15:14:22
tags:
- React Native
categories: [React Native]
---
React Native问题记录

<!-- more -->

1. Could not get BatchedBridge, make sure your bundle is packaged correctly
 在package.json中的"scripts"中添加
 ```JavaScript
 "bundle-android":"react-native bundle --platform android --dev false --entry-file index.android.js --bundle-output android/app/src/main/assets/index.android.bundle --sourcemap-output android/app/src/main/assets/index.android.map --assets-dest android/app/src/main/res/",
 ```
 如果没有assets目录,手动添加下,不过运行时没有效果, 在cmd中手动执行下, assets目录中会多出几个文件, 即可解决这个问题.
 
2. 

