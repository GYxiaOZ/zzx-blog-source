---
title: eslint 禁用规则
date: 2019-05-17 09:52:35
categories: [work]
tags: [tips]
---

## 块禁用

```javascript
/* eslint-disable */
alert('foo');
/* eslint-enable */
```

## 块指定的规则禁用

```javascript
/* eslint-disable no-alert, no-console */
alert('foo');console.log('bar');
/* eslint-enable no-alert, no-console */
```

<!-- more -->

## 整个文件禁用

```javascript
/* eslint-disable */
alert('foo');
```

## 整个文件禁用指定的规则

```javascript
/* eslint-disable no-alert */
alert('foo');
```

## 行注释

```javascript
alert('foo'); // eslint-disable-line
// eslint-disable-next-line
alert('foo');
```

## 行禁用禁用指定的规则

```javascript
alert('foo'); // eslint-disable-line no-alert, quotes, semi
// eslint-disable-next-line no-alert, quotes, semi
alert('foo');
```

## 禁用插件规则

```javascript
foo(123); // eslint-disable-line 插件名/规则名
```