---
title: learn-ts4
date: 2019-05-12 21:30:33
categories: [学习笔记, typescript]
tags: [typescript]
---

## 函数类型

```typescript
function add(x: number, y: number): number {
  return x + y
}
```

## 完整函数类型

```typescript
let myAdd: (baseValue: number, increment: number) => number =
function(x: number, y: number): number {
  return x + y
}
```

ts有类型推断，不需要写的那么完整，上面的例子可以只写左边或右边