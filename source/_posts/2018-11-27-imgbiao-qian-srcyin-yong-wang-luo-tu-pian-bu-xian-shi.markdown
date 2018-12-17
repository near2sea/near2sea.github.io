---
layout: post
title: img标签src引用网络图片不显示
date: '2018-11-27 20:29:26 +0800'
comments: true
categories: h5
abbrlink: 34789
---

### 在src中引用网络图片无法显示，这因为在引用链接时,浏览器对地址发起请求加上了refre这个请求头,而有些服务器能根据refre反盗所以引用失败返回403。

解决方法：
```html
<meta name="referrer" content="no-referrer">
```
