---
title: ConstraintLayout
date: 2017-05-09 15:22:56
tags:
- Android
- 控件
categories: [Android]
---
ConstraintLayout约束布局 学习

<!-- more -->

# Constraint 系统概述

Layout 引擎使用Constraints指定每个widget来决定他们在Layout中的位置。

# ConstraintLayout应用

* 载入constraint-layout依赖

```
dependencies {
...
compile 'com.android.support.constraint:constraint-layout:1.0.0-alpha2'
}
```

* 配置xml文件

```
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

</android.support.constraint.ConstraintLayout>
```


