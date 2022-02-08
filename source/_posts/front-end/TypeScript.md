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
- [TypeScript Handbook（中文版）](https://zhongsp.gitbooks.io/typescript-handbook/content/)
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

## 类型简介

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

```TypeScript
let a: number|string = 123
```

#### 字符串字面量

字符串字面量类型用来约束取值只能是某几个字符串中的一个。

```TypeScript
type EventNames = 'click' | 'scroll' | 'mousemove';
function handleEvent(ele: Element, event: EventNames) {
  // do something
}
handleEvent(document.getElementById('hello'), 'scroll');  // 没问题
handleEvent(document.getElementById('world'), 'dblclick'); // 报错，event 不能为 'dblclick'
```

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

#### 额外的属性检查

参考：[TypeScript 额外的属性检查 - SegmentFault 思否](https://segmentfault.com/a/1190000022418050)

> 发现报错的第一个例子中，我们传入函数的是一个类似于{ color: "black", opacity: 0.5 }的对象字面量（object literal），而在第二个不报错的例子中，我们传入的是一个类似于 myObj 的变量（variable）。
>
> 由此我们可以看到，TypeScript 中额外的属性检查只会应用于对象字面量场景，所以，在 TS 的官方测试用例里面，我们看到的都是 objectLiteralExcessProperties.ts。
>
> 用变量的情况下，即使他是类似于 function printLabel(labeledObj: LabeledValue)这样函数中的一个参数，也不会触发额外属性检查，因为他会走另一个逻辑：类型兼容性
>
> 回到上面的例子，在定义 myObj 的时候，并没有指定它的类型，所以 TS 会推断他的类型为{ size: number; label: string; }。当他作为参数传入 printLabel 函数时，ts 会比较它和 LabelledValue 是否兼容，因为 LabelledValue 中的 label 属性的，myObj 也存在，所以他们是兼容的，这就是最上面提到的鸭式辨型法。
>
> 1. 每个对象字面量在初始化的时候都被认为是新鲜（fresh）的
>
> 2. 当一个新鲜的对象字面量在赋值给一个非空类型的变量，或者作为一个非空类型的参数时，如果这个对象字面量里没有那个非空类型中指定的属性，就会报错
>
> 3. 在类型断言后，或者对象字面量的类型被拓展后，新鲜度会消失，此时对象字面量就不再新鲜

### 函数类型

#### 函数声明

```Typescript
function sum(x: number, y: number): number {
  return x + y;
}
```

输入输出都要考虑到，输入的参数多了少了都不行。

#### 函数表达式

可以通过赋值操作进行类型推论

```TypeScript
let mySum = function (x: number, y: number): number {
  return x + y;
};
```

使用了类型推断，想自己手写按照下面的写

```TypeScript
let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
};

// 箭头函数
let mySum: (x: number, y: number) => number = (x: number, y: number): number => {
  return x + y;
};

```

#### 使用接口定义函数

```TypeScript
interface SearchFunc {
    (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    return source.search(subString) !== -1;
}
```

#### 可选参数、默认值、剩余参数

- 可选参数后不允许出现必须参数
- 有了默认值就可以在后面加必须参数了
- 剩余参数用 ... 操作符 相当于数组

```TypeScript
function buildName(firstName: string, lastName?: string){}
function buildName(firstName: string, lastName: string = 'Cat'){}
function push(array: any[], ...items: any[]){}
```

#### 重载

函数重载：**函数项名称相同** 但输入输出类型或个数不同的子程序，它可以简单地称为一个单独功能可以执行多项任务的能力。

```TypeScript
// 重载签名
function add(x:string,y:string):string;
function add(x:number, y:number):number;
//实现签名 对外不可见
function add(x:string|number, y: number|string): number | string
```

### 类型别名

给已有的类型起个新名字，一般要大写

```TypeScript
type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;
```

### 类

#### 接口的关系

### 泛型

### 声明合并

## 编写`d.ts`文件
