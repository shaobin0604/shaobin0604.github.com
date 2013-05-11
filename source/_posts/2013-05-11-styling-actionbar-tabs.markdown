---
layout: post
title: "Styling ActionBar Tabs"
date: 2013-05-11 23:12
comments: true
categories: [Android, ActionBar]
---

## 问题说明

修改 `ActionBar` `Tabs` 模式在 `Theme.Holo.Light` 下的样式，以前是这个样子滴

{% img /images/actionbar_tabs_old.png 480 %}

现要改成如下样式

{% img /images/actionbar_tabs_new.png 480 %}

## 实现方式

打开 SDK 里的 Theme 文件 `<ANDROID_SDK>/platforms/android-4.2/data/res/values/themes.xml`,与 `ActionBar` 相关的属性如下：

```
<!-- Action bar styles -->
<item name="actionDropDownStyle">@android:style/Widget.Holo.Light.Spinner.DropDown.ActionBar</item>
<item name="actionButtonStyle">@android:style/Widget.Holo.Light.ActionButton</item>
<item name="actionOverflowButtonStyle">@android:style/Widget.Holo.Light.ActionButton.Overflow</item>
<item name="actionModeBackground">@android:drawable/cab_background_top_holo_light</item>
<item name="actionModeSplitBackground">@android:drawable/cab_background_bottom_holo_light</item>
<item name="actionModeCloseDrawable">@android:drawable/ic_cab_done_holo_light</item>
<item name="actionBarTabStyle">@style/Widget.Holo.Light.ActionBar.TabView</item>
<item name="actionBarTabBarStyle">@style/Widget.Holo.Light.ActionBar.TabBar</item>
<item name="actionBarTabTextStyle">@style/Widget.Holo.Light.ActionBar.TabText</item>
<item name="actionModeStyle">@style/Widget.Holo.Light.ActionMode</item>
<item name="actionModeCloseButtonStyle">@style/Widget.Holo.Light.ActionButton.CloseMode</item>
<item name="android:actionBarStyle">@android:style/Widget.Holo.Light.ActionBar.Solid</item>
<item name="actionBarSize">@dimen/action_bar_default_height</item>
<item name="actionModePopupWindowStyle">@android:style/Widget.Holo.Light.PopupWindow.ActionMode</item>
<item name="actionBarWidgetTheme">@null</item>
```

与 `Tab` 相关的属性分析是

* actionBarTabBarStyle - 多个Tab的容器样式
* actionBarTabStyle - 单个Tab样式
* actionBarTabTextStyle - Tab上的字体风格

我们需要修改的样式有以下几部分

* TabBar 背景色
* 多个 Tab 之间的分隔符
* Tab 背景色

创建 `style.xml`

``` xml style.xml
<resources>
    <style name="MyActionBarTabBarStyle" parent="@android:style/Widget.Holo.Light.ActionBar.TabBar">
        <item name="android:background">@drawable/actionbar_tabs_bar_bg</item>
        <item name="android:divider">@drawable/actionbar_divider</item>
    </style>

    <style name="MyActionBarTabStyle" parent="@android:style/Widget.Holo.Light.ActionBar.TabView">
        <item name="android:background">@drawable/actionbar_tabs_bg</item>
    </style>

    <style name="AppTheme.Holo.Light" parent="@android:style/Theme.Holo.Light">
        <item name="android:actionBarTabBarStyle">@style/MyActionBarTabBarStyle</item>
        <item name="android:actionBarTabStyle">@style/MyActionBarTabStyle</item>
    </style>
</resources>
```

`@drawable/actionbar_tabs_bg` 是 `StateListDrawable`

``` xml actionbar_tabs_bg.xml 
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">

    <!-- Selected states -->
    <item android:drawable="@drawable/actionbar_tabs_bg_pressed" android:state_focused="false" android:state_pressed="false" android:state_selected="true"/>
    <!-- Focused states -->
    <item android:drawable="@drawable/actionbar_tabs_bg_pressed" android:state_focused="true" android:state_pressed="false" android:state_selected="false"/>
    <item android:drawable="@drawable/actionbar_tabs_bg_pressed" android:state_focused="true" android:state_pressed="false" android:state_selected="true"/>
    <!-- Pressed -->
    <item android:drawable="@drawable/actionbar_tabs_bg_pressed" android:state_pressed="true"/>

    <item android:drawable="@android:color/transparent" android:state_focused="false" android:state_pressed="false" android:state_selected="false"/>

</selector>
```


## 参考
* [Styling the ActionBar – Part 1](http://blog.stylingandroid.com/archives/1240)
* [Customizing the Action Bar](http://android-developers.blogspot.co.uk/2011/04/customizing-action-bar.html)
