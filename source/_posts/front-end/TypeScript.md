---
title: TypeScript 笔记
date: 2022-01-19 17:19:00
tags:
  - TypeScript
categories:
  - [前端, TypeScript]
---

教程来自

- [TypeScript 中文手册](https://typescript.bootcss.com/)
- [TypeScript 入门教程](https://ts.xcatliu.com/)
<!--more-->

## 简介

- 静态类型(编译时检测类型)
  - JS 是动态类型 运行时检测
- 弱类型(允许隐式类型转换)
  - 可以使用 ESLint 提供更严格约束

```shell
npm install -g typescript

tsc hello.ts
```

TypeScript 只会在编译时对类型进行静态检查，如果发现有错误，编译的时候就会报错。
TypeScript 编译的时候即使报错了，还是会生成编译结果.

## 基础

### 简单类型

#### 原始数据类型

```typescript
let name: string = "Tom";
let age: number = 25;
let isStudent: boolean = true;
```

`new Boolean(1)` 是 `Boolean` 对象 `Boolean(1)` 才是布尔型

- `boolean`
- `number`
- `string`
- `null` 和 `undefined`
  - 是所有类型的子类型，可以赋值给任何类型
  - 加上 `strictNullChecks` 赋值给其他类型会报错

#### any

任何类型都可以赋值给 any 类型，any 类型也可以赋值给任何类型，any 类型可以执行所有函数。

#### unknow

unknown 类型的变量只能赋值给 any 和 unknown，不能赋给其他类型，unknown 类型的所有函数都会报错。

```typescript
let foo: any = 123;
console.log(foo.msg); // 符合TS的语法
let a_value1: unknown = foo; // OK
let a_value2: any = foo; // OK
let a_value3: string = foo; // OK

let bar: unknown = 222; // OK
console.log(bar.msg); // Error
let k_value1: unknown = bar; // OK
let K_value2: any = bar; // OK
let K_value3: string = bar; // Error
```

#### void

没有任何类型，用于没有返回值的函数

```typescript
function hello(): void {
  console.log("hello world");
}
```

#### never

永不存在的值，用于永远不会正常返回的函数

```typescript
function error(message: string): never {
  throw new Error(message);
}

function infiniteLoop(): never {
  while (true) {}
}
```

### 类型断言

告诉编译器，“相信我，我知道自己在干什么”。
相当于类型转换，但是不进行特殊的数据检查和解构。

```Typescript
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;

let someValue: any = "this is a string";
let strLength: number = (someValue as string).length; // 支持JSX
```

### 类型推断

一个变量没有显示指定类型，声明时就赋值时确定类型，声明时不赋值是 any 类型。

```typescript
let name = "John"; // string
let age;
age = 20; // any
```

### 其他类型

#### Array

`类型[]` 或 `Array<类型>`

#### enum

```typescript
enum Direction {
  NORTH,
  SOUTH,
  EAST,
  WEST,
}
let dir: Direction = Direction.NORTH;

// 指定值
enum Direction {
  NORTH = 5,
  SOUTH, \\6
  EAST, \\7
  WEST, \\8
}
let dir: Direction = Direction.NORTH;

// 指定string要显式指定所有的
enum Direction {
  NORTH = "NORTH",
  SOUTH = "SOUTH",
  EAST = "EAST",
  WEST = "WEST",
}
let dir: Direction = Direction.NORTH;
```

#### tuple

析构声明赋值

```typescript
// 一次必须全指定
let tupleType: [string, boolean];
tupleType = ["Semlinker", true];
```

#### 联合类型

可以是多种类型中的一种，只能访问共有属性和方法，如果被赋值会自动推断类型。

### 接口

#### 接口定义

定义复杂的类型用于判定，只要传入的对象包含字段就通过（李斯科夫替换法则）
鸭式判定：“当我看到一只鸟，它走路像鸭子、游泳像鸭子、叫声像鸭子，我就称其为鸭子。”
“如果它看起来像鸭子，叫起来也像鸭子，但是它需要电池才能工作，那么你的抽象大概是做错了。”

```typescript
function printLabel(labelledObj: { label: string }) {
  console.log(labelledObj.label);
}

let myObj = { size: 10, label: "Size 10 Object" };
printLabel(myObj);

// ------------------

interface LabelledValue {
  label: string;
}

function printLabel(labelledObj: LabelledValue) {
  console.log(labelledObj.label);
}

let myObj = { size: 10, label: "Size 10 Object" };
printLabel(myObj);
```

编译器只会检查那些必需的属性是否存在，并且其类型是否匹配。

#### 可选属性

接口里的属性不全都是必需的。 有些是只在某些条件下存在，或者根本不存在。

```typescript
interface SquareConfig {
  color?: string;
  width?: number;
}
```

#### 只读属性

```typescript
interface Point {
  readonly x: number;
  readonly y: number;
}

let p1: Point = { x: 10, y: 20 };
p1.x = 5; // error!
```

定义了就改不了
