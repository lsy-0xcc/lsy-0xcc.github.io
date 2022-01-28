---
title: JavaScript 扩展运算符(...) 解构赋值 函数调用
date: 2022-01-24 08:21:23
tags: 

  - JavaScript
categories: 
  - [前端, JavaSript]
---


## 长求总

- 解构赋值
- 函数的 rest 参数和 spread 语法
- 适用于对象和数组
- 用来折腾复杂的对象或者数组，或者用来定义参数特别多特别乱的函数

<!--more-->

## 解构赋值

### 数组解构

```JavaScript
let[firstName, secondName] = ["John", "Doe"];
```

- 意味着复制一个新的，不破坏
- 可以用逗号忽略

  ```JavaScript
  // 不需要第二个元素
  let [firstName, , title] = ["Julius", "Caesar", "Consul", "of the Roman Republic"];
  alert( title ); // Consul
  ```

- 等号右侧可以是任何可迭代对象
- 赋值给等号左侧的任何内容
- 与 .entries() 方法进行循环操作
- 交换变量值的技巧`[guest, admin] = [admin, guest];`

#### 剩余元素

这里用了**三个点**，剩下的东西扔数组里

```JavaScript
let [name1, name2, ...rest] = ["Julius", "Caesar", "Consul", "of the Roman Republic"];

alert(name1); // Julius
alert(name2); // Caesar

// 请注意，`rest` 的类型是数组
alert(rest[0]); // Consul
alert(rest[1]); // of the Roman Republic
alert(rest.length); // 2
```

#### 默认值

```JavaScript
// 默认值
let [name = "Guest", surname = "Anonymous"] = ["Julius"];

alert(name);    // Julius（来自数组的值）
alert(surname); // Anonymous（默认值被使用了）
```

#### 数组展开

这里用了**三个点**，相当于把数组打开成单个元素

```JavaScript
let array = [1,2,3]
let newArray = [4,5,...array,6,..."hello"] // [4, 5, 1, 2, 3, 6, 'h', 'e', 'l', 'l', 'o']
```

### 对象解构

```JavaScript
let {var1, var2} = {var1:…, var2:…}
// var1 var2 可以换位置，不影响结果。

// 自定义变量名
let options = {
  title: "Menu",
  width: 100,
  height: 200
};
// { 对象属性: 变量名 }
let {width: w, height: h, title} = options // ;
```

#### 剩余元素

```JavaScript
let options = {
  title: "Menu",
  width: 100,
  height: 200
};
let {title, ...shape} = options;
```

#### 默认值

```JavaScript
let options = {
  title: "Menu"
};
let {width = 100, height = 200, title} = options;
```

#### 对象展开

这里用了**三个点**，相当于把对象摊平

```JavaScript
let shape = {width:16, height:9}
let square = {title:"I'm square", ...shape} // {title: "I'm square", width: 16, height: 9}
```

## 关于函数

### Rest 参数与 Spread 语法

- 若 ... 出现在函数参数列表的最后，那么它就是 rest 参数，它会把参数列表中剩余的参数收集到一个数组中。
- 若 ... 出现在函数调用或类似的表达式中，那它就是 spread 语法，它会把一个数组展开为列表。

### 智能函数参数

和解构对象相同

```JavaScript
let options = {
  title: "My menu",
  items: ["Item1", "Item2"]
};

function showMenu({
  title = "Untitled",
  width: w = 100,  // width goes to w
  height: h = 200, // height goes to h
  items: [item1, item2] // items first element goes to item1, second to item2
}) {
  alert( `${title} ${w} ${h}` ); // My Menu 100 200
  alert( item1 ); // Item1
  alert( item2 ); // Item2
}

showMenu(options);
```
