---
title: typescript学习(5) 泛型
date: 2019-05-16 21:56:03
categories: [学习笔记, typescript]
tags: [typescript]
---

泛型为定义提供了重用性，在创建大型系统时为你提供了十分灵活的功能

## 基础实例

创建一个 `identity` 函数，它会返回任何传入值，如果不用泛型

```typescript
function identity(arg: any): any {
  return arg;
}
```

但是使用 `any` 会导致 ts 不提供类型检查，我们需要知道传入类型和返回类型是相同的，所以需要使用泛型，泛型是一种 _类型变量_ ，它值用于表示类型而不是值

```typescript
function identity<T>(arg: T): T {
  return arg;
}
```

<!-- more -->

我们给 `identity` 添加了类型变量 `T`。 `T` 帮助我们捕获用户传入的类型（比如：`number`），之后我们就可以使用这个类型。 之后我们再次使用了 `T` 当做返回值类型。现在我们可以知道参数类型与返回值类型是相同的了。这允许我们跟踪函数里使用的类型的信息。

定义了泛型之后，有两种方法使用

```typescript
let output = identity<string>('myString');
let output1 = identity('myString');
```

第一种明确制定了 `T` 是 `string` 类型，并作为一个参数传给函数
第二种利用了 _类型推论_ ，ts 会根据参数自动确定 T 的类型
一般同第二种就好了，当 ts 无法推断时，用第一种

## 泛型变量

泛型变量可以当做类型的一部分使用

```typescript
function loggingIdentity<T>(arg: T): T {
  console.log(arg.length); // error: Property 'length' does not exist on type 'T'.
  return arg;
}
```

类型变量 `T` 代表的是任意类型，所以使用这个函数的人可能传入的是个数字，而数字是没有 `.length` 属性的，ts 会报错

```typescript
function loggingIdentity<T>(arg: T[]): T[] {
  console.log(arg.length);
  return arg;
}
```

用以上方式写，参数 `arg` 是一个数组，所以有 length 属性，此时 `T` 表示数组元素的类型

## 泛型类型

### 函数

```typescript
function identity<T>(arg: T): T {
  return arg;
}
let myIdentity: <U>(arg: U) => U = identity;
```

可以使用不同的泛型参数名，只要在数量上和使用方式上能对应上就可以。

### 泛型接口

```typescript
interface GenericIdentityFn {
  <T>(arg: T): T;
}
// 编辑器会建议修改为类型别名的写法
// type GenericIdentityFn = <T>(arg: T) => T

function identity<T>(arg: T): T {
  return arg;
}

let myIdentity: GenericIdentityFn = identity;
```

我们还可以把泛型参数当作整个接口的一个参数。 这样我们就能清楚的知道使用的具体是哪个泛型类型（比如： `Dictionary<string>` 而不只是 `Dictionary` ）。这样接口里的其它成员也能知道这个参数的类型了

```typescript
interface GenericIdentityFn<T> {
  (arg: T): T;
}
// 编辑器会建议修改为：
// type GenericIdentityFn<T> = (arg: T) => T;

function identity<T>(arg: T): T {
  return arg;
}

let myIdentity: GenericIdentityFn<number> = identity;
```

注意，我们的示例做了少许改动。 不再描述泛型函数，而是把非泛型函数签名作为泛型类型一部分。 当我们使用 `GenericIdentityFn` 的时候，还得传入一个类型参数来指定泛型类型（这里是： `number` ），锁定了之后代码里使用的类型。对于描述哪部分类型属于泛型部分来说，理解何时把参数放在调用签名里和何时放在接口上是很有帮助的

## 泛型类

泛型类看上去与泛型接口差不多。 泛型类使用 `<>` 括起泛型类型，跟在类名后面

```typescript
class GenericNumber<T> {
  zeroValue: T;
  add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) {
  return x + y;
};
```

与接口一样，直接把泛型类型放在类后面，可以帮助我们确认类的所有属性都在使用相同的类型。

类有两部分：静态部分和实例部分。 泛型类指的是实例部分的类型，所以类的静态属性不能使用这个泛型类型

## 泛型约束

```typescript
function loggingIdentity<T>(arg: T): T {
  console.log(arg.length); // error: Property 'length' does not exist on type 'T'
  return arg;
}
```

要修正上面的例子中的错误，可以使用到泛型约束

我们定义一个接口来描述约束条件，创建一个包含 `.length` 属性的接口，使用这个接口和 `extends` 关键字来实现约束

```typescript
interface Lengthwise {
  length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
  console.log(arg.length); // OK
  return arg;
}

loggingIdentity(3); // Error
loggingIdentity({ length: 10, value: 3 }); // OK
```

我们需要传入符合约束类型的值，必须包含必须的属性

## 在泛型约束中使用类型参数

你可以声明一个类型参数，且它被另一个类型参数所约束。 比如，现在我们想要用属性名从对象里获取这个属性。 并且我们想要确保这个属性存在于对象 `obj` 上，因此我们需要在这两个类型之间使用约束。

```typescript
function getProperty<T, K extends keyof T>(obj: T, key: K) {
  return obj[key];
}

let x = { a: 1, b: 2, c: 3, d: 4 };

getProperty(x, 'a'); // okay
getProperty(x, 'm'); // error
```
