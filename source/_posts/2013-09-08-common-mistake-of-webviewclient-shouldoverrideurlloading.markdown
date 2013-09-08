---
layout: post
title: "Common Mistake of WebViewClient shouldOverrideUrlLoading"
date: 2013-09-08 21:51
comments: true
categories: [Android, WebView]
---

## 需求描述

在使用 `WebView` 的项目中，一个常见的需求是将页面内的链接跳转限制在 `WebView` 内，而不是使用外部浏览器打开，但 `WebView` 的默认行为是将链接点击事件作为 `Intent` 发送给系统，由系统决定如何处理（通常的行为是使用浏览器打开或是弹出浏览器选择对话框），那么如何实现期望的效果呢？ 

## 实现方案

文章[关于用WebView或手机浏览器打开连接问题](http://blog.csdn.net/chenshijun0101/article/details/7045145)给出了一种使用 `WebViewClient#shouldOverrideUrlLoading`方法的方案可以达到效果，但其使用该方法的方式是错误的，且被广泛传播。因此，有必要在此澄清一下。

`WebView#shouldOverrideUrlLoading` 的 api doc 如下

```
public boolean shouldOverrideUrlLoading (WebView view, String url)

Added in API level 1
Give the host application a chance to take over the control when a > new url is about to be loaded in the current WebView. If WebViewClient is not provided, by default WebView will ask Activity Manager to choose the proper handler for the url. If WebViewClient is provided, return true means the host application handles the url, while return false means the current WebView handles the url. This method is not called for requests using the POST "method".

Parameters
view	The WebView that is initiating the callback.
url	The url to be loaded.
Returns
True if the host application wants to leave the current WebView and handle the url itself, otherwise return false.
```

翻译一下，三种情况：

1. 若没有设置 `WebViewClient` 则在点击链接之后由系统处理该 url，通常是使用浏览器打开或弹出浏览器选择对话框。
2. 若设置 `WebViewClient` 且该方法返回 *true* ，则说明由应用的代码处理该 url，`WebView` 不处理。
3. 若设置 `WebViewClient` 且该方法返回 *false*，则说明由 `WebView` 处理该 url，即用 `WebView` 加载该 url。

因此，开篇的需求使用如下方法即可实现：

> 设置 `WebViewClient` 且在其 `shouldOverrideUrlLoading` 方法返回 *false*

示例程序见 [WebViewClientTest](https://github.com/shaobin0604/miscandroidapps/tree/master/WebViewClientTest)



