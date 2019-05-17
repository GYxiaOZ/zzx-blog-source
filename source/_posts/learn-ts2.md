---
title: typescript学习(2) 接口
date: 2019-05-09 21:45:30
categories: [学习笔记, typescript]
tags: [typescript]
---

## 简单介绍

TypeScript 的核心原则之一是对值所具有的结构进行类型检查，接口就是对解构的一种描述

```typescript
interface Square {
  color: string,
  area?: number // 可选
}
```

## 只读属性

在属性名前用 readonly 来指定只读属性

```typescript
interface Point {
  readonly x: number
  readonly y: number
}
let p1: Point = { x: 10, y: 20 }
p1.x = 5 // error!
```

<!-- more -->

TypeScript 具有 ReadonlyArray<T> 类型，它与 Array<T> 相似，只是把所有可变方法去掉了，因此可以确保数组创建后再也不能被修改

```typescript
let arr: ReadonlyArray<number> = [1, 2, 3, 4]
arr[0] = 66 // error!
let arr1: number[] = [1, 2, 3]
arr1 = arr // error!
```

可以看到就算把整个 ReadonlyArray 赋值到一个普通数组也是不可以的。 但是你可以用类型断言重写

```typescript
arr1 = arr as number[]
```

## 额外的属性检查

```typescript
interface SquareConfig {
  color?: string
  width?: number
  [propName: string]: any
}
```

### 函数接口

```typescript
interface Func {
  (param: string): boolean
}
```

对于函数类型的类型检查来说，函数的参数名不需要与接口里定义的名字相匹配

```typescript
let func: Func
func = function(src: string): boolean {
  let result = src.search('123');
  return result > -1
}
```

## 可索引的类型

与使用接口描述函数类型差不多，我们也可以描述那些能够“通过索引得到”的类型，比如 `a[10]` 或 `ageMap['daniel']`。 可索引类型具有一个 索引签名，它描述了对象索引的类型，还有相应的索引返回值类型。 让我们看一个例子：

```typescript
interface StringArray {
  [index: number]: string
}

let myArray: StringArray
myArray = ['Bob', 'Fred']

let myStr: string = myArray[0]
```

上面例子里，我们定义了 `StringArray` 接口，它具有索引签名。 这个索引签名表示了当用 `number` 去索引 `StringArray` 时会得到 `string` 类型的返回值。

TypeScript 支持两种索引签名：字符串和数字。 可以同时使用两种类型的索引，但是数字索引的返回值必须是字符串索引返回值类型的子类型。 这是因为当使用 `number` 来索引时，JavaScript 会将它转换成`string` 然后再去索引对象。 也就是说用 `100`（一个 `number`）去索引等同于使用`'100'`（一个 `string` ）去索引，因此两者需要保持一致。

```typescript
class Animal {
  name: string
}
class Dog extends Animal {
  breed: string
}

// 错误：使用数值型的字符串索引，有时会得到完全不同的Animal!
interface NotOkay {
  [x: number]: Animal
  [x: string]: Dog
}
```

字符串索引签名能够很好的描述 `dictionary` 模式，并且它们也会确保所有属性与其返回值类型相匹配。 因为字符串索引声明了 `obj.property` 和 `obj['property']` 两种形式都可以。 下面的例子里， `name` 的类型与字符串索引类型不匹配，所以类型检查器给出一个错误提示：

```typescript
interface NumberDictionary {
  [index: string]: number;
  length: number;    // 可以，length是number类型
  name: string       // 错误，`name`的类型与索引类型返回值的类型不匹配
}
```

最后，你可以将索引签名设置为只读，这样就防止了给索引赋值：

```typescript
interface ReadonlyStringArray {
  readonly [index: number]: string;
}
let myArray: ReadonlyStringArray = ['Alice', 'Bob'];
myArray[2] = 'Mallory'; // error!
```

## 类实例接口

`implements` 关键字的含义是“实现”

```typescript
interface ClockInterface {
  currentTime: Date
  setTime(d: Date):void
}

class Clock implements ClockInterface {
  currentTime: Date
  setTime(d: Date) {
    this.currentTime = d
  }
  constructor(h: number, m: number) { }
}
```

## 类类型接口

```typescript
interface ClockConstructor {
 new (hour: number, minute: number)
}
```

## 继承接口

```typescript
interface Shape {
  color: string
}
interface Square extends Shape {
  sideLength: number
}
let square: Square = {
  color: 'red',
  sideLength: 10
};
```

一个接口可以继承多个接口，创建出多个接口的合成接口

```typescript
interface Shape {
  color: string
}

interface PenStroke {
  penWidth: number
}

interface Square extends Shape, PenStroke {
  sideLength: number
}
let square: Square = {
  color: 'red',
  penWidth:20,
  sideLength: 10
};
```

## 接口继承类

当接口继承了一个类类型时，它会继承类的成员但不包括其实现。 就好像接口声明了所有类中存在的成员，但并没有提供具体实现一样。 接口同样会继承到类的 `private` 和 `protected` 成员。 这意味着当你创建了一个接口继承了一个拥有私有或受保护的成员的类时，这个接口类型只能被这个类或其子类所实现，使用场景不多

```typescript
class Control {
  private state: any
}

interface SelectableControl extends Control {
  select(): void
}

class Button extends Control implements SelectableControl {
  select() { }
}

class TextBox extends Control {
  select() { }
}

// Error：“ImageC”类型缺少“state”属性。
class ImageC implements SelectableControl {
  select() { }
}
```

在上面的例子里，`SelectableControl` 包含了 `Control` 的所有成员，包括私有成员 `state`。 因为 `state` 是私有成员，所以只能够是 `Control` 的子类们才能实现 `SelectableControl` 接口。 因为只有 `Control` 的子类才能够拥有一个声明于`Control` 的私有成员 `state`，这对私有成员的兼容性是必需的。

在 `Control` 类内部，是允许通过 `SelectableControl` 的实例来访问私有成员 `state` 的。 实际上，`SelectableControl` 接口和拥有 `select` 方法的 `Control` 类是一样的。`Button`和 `TextBox` 类是 `SelectableControl` 的子类（因为它们都继承自`Control` 并有 `select` 方法），但 `ImageC` 类并不是这样的。
