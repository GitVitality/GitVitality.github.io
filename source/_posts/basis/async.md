---
title: JavaScript异步处理
date: 2019-05-07 09:00:00
categories: basis
---

## 遇到问题

1. 在 promise 之间传递数据

## 分析问题（异步处理的发展）

### 回调函数

缺点：陷入回调函数嵌套的地狱

### promise

优点：

1. 解决了回调函数嵌套的地狱问题
2. 有明确的状态管理

缺点：

1. 代码不清晰
2. 在业务当中遇到了需要在 promise 之间传递共享数据的需求

### aync await

优点：

1. 代码逻辑结构清晰
2. 可以采用 try catch 捕获错误

## 解决问题

1. 尽量采用 async await 函数，但是需要在 created、mounted 等生命周期中进行异步处理的话，created、mounted 函数写成异步函数有些奇怪。
2. 注意错误捕获，如果使用 promise，一定要 catch 错误；使用 await，需要 try catch 错误。注意 try catch 只能捕获同步处理的错误，不能捕获 promise 内部的错误 😯。

## 参考链接

[Promise 中多个回调函数之间的数据传递](https://juejin.im/post/59970a73f265da249150f490)
[clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript#asyncawait-are-even-cleaner-than-promises)
