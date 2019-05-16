---
title: typescript学习(4) 函数
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

## 可选参数和默认参数

没传参的时候，它的值就是 `undefined`，可选参数必须跟在必须参数后面

```typescript
let myAdd: (baseValue: number, increment?: number) => number
```

默认初始化值的参数：当用户没有传递这个参数或传递的值是 undefined 时，为参数提供一个默认值

```typescript
function buildName(firstName: string, lastName = 'Smith'): string {
  return firstName + ' ' + lastName
}
```

## 剩余参数

想同时操作多个参数，或者你并不知道会有多少参数传递进来。 在 JavaScript 里，你可以使用 `arguments` 来访问所有传入的参数。

在 TypeScript 里，你可以把所有参数收集到一个变量里：

```typescript
function buildName(firstName: string, ...restOfName: string[]): string {
  return firstName + ' ' + restOfName.join(' ')
}

let employeeName = buildName('Joseph', 'Samuel', 'Lucas', 'MacKinzie')
```

## this

TypeScript 能通知你错误地使用了 this 的地方

### this 和箭头函数

箭头函数能保存函数创建时的 `this` 值，而不是调用时的值

### this 参数

可以提供一个显式的 `this` 参数。 `this` 参数是个假的参数，它出现在参数列表的最前面：

```typescript
function f(this: void) {
  // 确保“this”在此独立函数中不可用
}
```

一个定义 `this` 参数的例子

```typescript
interface Card {
  suit: string
  card: number
}

interface Deck {
  suits: string[]
  cards: number[]

  createCardPicker (this: Deck): () => Card
}

let deck: Deck = {
  suits: ['hearts', 'spades', 'clubs', 'diamonds'],
  cards: Array(52),
  // NOTE: 函数现在显式指定其被调用方必须是 deck 类型
  createCardPicker: function (this: Deck) {
    return () => {
      let pickedCard = Math.floor(Math.random() * 52)
      let pickedSuit = Math.floor(pickedCard / 13)
      // console.log(this.a) // Property 'a' does not exist on type 'Deck'
      return {suit: this.suits[pickedSuit], card: pickedCard % 13}
    }
  }
}

let cardPicker = deck.createCardPicker()
let pickedCard = cardPicker()

console.log('card: ' + pickedCard.card + ' of ' + pickedCard.suit)
```

## 函数重载

JavaScript 本身是个动态语言。JavaScript 里函数根据传入不同的参数而返回不同类型的数据的场景是很常见的。
方法是为同一个函数提供多个函数类型定义来进行函数重载。 编译器会根据这个列表去处理函数的调用。

```typescript
let suits = ['hearts', 'spades', 'clubs', 'diamonds']

function pickCard(x: {suit: string; card: number }[]): number
function pickCard(x: number): {suit: string; card: number }

function pickCard(x): any {
  if (Array.isArray(x)) {
    let pickedCard = Math.floor(Math.random() * x.length)
    return pickedCard
  } else if (typeof x === 'number') {
    let pickedSuit = Math.floor(x / 13)
    return { suit: suits[pickedSuit], card: x % 13 }
  }
}

let myDeck = [
  { suit: 'diamonds', card: 2 },
  { suit: 'spades', card: 10 },
  { suit: 'hearts', card: 4 }
]
let pickedCard1 = myDeck[pickCard(myDeck)];
let pickedCard2 = pickCard(15)
let pickedCard3 = pickCard('aaa') // error 类型“"aaa"”的参数不能赋给类型“number”的参数
```

重载的 `pickCard` 函数在调用的时候会进行正确的类型检查