---
title: 多iconfont项目共用方案
date: 2019-05-08 10:11:51
categories: [css]
tags: [tips]
---

> 大多数情况下一个项目只需要一个iconfont项目，但是在某些特殊情况下，需要不止一个iconfont项目共同使用，这里提供一个方案：

## 1. 在项目中设置单独的前缀和FontFamily（最重要的一步）

![img](http://img.xiaoz.site/20190508101232.png)
![img](http://img.xiaoz.site/20190508101248.png)

<!-- more -->

## 2. 按需求在项目中用Unicode 或者 Font class 的方式引入即可

### (1) Unicode方式：

![img](http://img.xiaoz.site/20190508101258.png)

使用

![img](http://img.xiaoz.site/20190508101307.png)

### (2) Font class 方式：

![img](http://img.xiaoz.site/20190508101313.png)

使用

![img](http://img.xiaoz.site/20190508101322.png)

### (3) 需要多色时使用Js方式：

![img](http://img.xiaoz.site/20190508101329.png)

使用

![img](http://img.xiaoz.site/20190508101336.png)

效果

![img](http://img.xiaoz.site/20190508101343.png)
