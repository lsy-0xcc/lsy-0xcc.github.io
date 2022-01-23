---
title: JavaScript 笔记
date: 2022-01-17 08:21:00
tags: 
  - JavaScript
categories: 
  - [前端, JavaSript]
---

教程来自

- [现代 JavaScript 教程](https://zh.javascript.info/)

<!--more-->

## 基础

1. 假值 `false` `0` `NaN` `undiefined` `null` `""`
2. `??` 空值合并运算符
   - `a??default` 若 `a` 是 `null` 或 `undefined` 返回 `default` 否则返回 `a`

## 对象

## 数据类型

1. 原始类型
   1. 只有 `string` `number` `bigint` `boolean` `symbol` `null` `undefined`
   2. 调用方法会自动包装，`null` `undefined` 无属性和方法
   3. 自动包装 `String()` `Number()` `Boolean()` 一般不要用
2. 数字
   1. 0b 0o 0x ...e...
   2. 常用函数
      - `number.toString(base)` 以 base 为进制转换
      - `parseInt(str [,radix])` radix是进制
      - Math.ceil Math.floor Math.round Math.trunc number.toFixed(n)
3. bigint 整形 n 结尾
4. 字符串
5. 数组

## 函数

## 对象属性配置

## 原型继承

## 类
