---
title: [Android6.0][RK3399] 隐藏导航栏或状态栏
tags: Rockchip
grammar_cjkRuby: true
---

[TOC]

## 导航栏与状态栏
![](https://ws1.sinaimg.cn/large/ba061518gy1fdf80niljjj20k00zkq7h)

最上面是状态栏 StatusBar
最下面是导航栏 NavagationBar

## 一、隐藏导航栏
### 方法一，修改资源文件代码
frameworks/base/core/res/res/values/dimens.xml
```xml
<dimen name="navigation_bar_height">48dp</dimen>
<!-- Height of the bottom navigation bar in portrait; often the same as @dimen/navigation_bar_height -->
<dimen name="navigation_bar_height_landscape">48dp</dimen>
<!-- Width of the navigation bar when it is placed vertically on the screen -->
```
将高度 48 改成 0
### 方法二，通过系统 property 来控制
路径 device/rockchip/rk3399/system.prop
```
qemu.hw.mainkeys=1
```
代码调用
rk3399/frameworks/base/services/core/java/com/android/server/policy/PhoneWindowManager.java
```java
mHasNavigationBar = res.getBoolean(com.android.internal.R.bool.config_showNavigationBar);
// Allow a system property to override this. Used by the emulator.
// See also hasNavigationBar().
String navBarOverride = SystemProperties.get("qemu.hw.mainkeys");
if ("1".equals(navBarOverride)) {
    mHasNavigationBar = false;
} else if ("0".equals(navBarOverride)) {
    mHasNavigationBar = true;
}
```

## 二、隐藏状态栏
### 修改资源文件代码
frameworks/base/core/res/res/values/dimens.xml
```xml
<dimen name="status_bar_height">24dp</dimen>
<!-- Height of the bottom navigation / system bar. -->
```
将高度 24 改成 0
