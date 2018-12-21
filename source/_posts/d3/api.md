---
title: d3基础api
date: 2018-08-27 09:13:13
tags: d3
categories: visualization
---

## API文档
[API Version3](https://github.com/d3/d3/blob/master/API.md)

### 元素操作
select、selectAll、append、insert、remove

### 数据操作
data、datum

### 比例尺
线性比例尺、序数比例尺
scaleLinear、scaleOrdinal(domain、range)

### 坐标轴
axiosLeft、axiosRight、axiosTop、axiosBottom

### 过渡
[transition](https://github.com/d3/d3-transition)
[ease](https://github.com/d3/d3-ease)

### 元素
update、enter、exit
```
update 部分的处理办法一般是：更新属性值
enter 部分的处理办法一般是：添加元素后，赋予属性值
exit 部分的绝大部分操作是删除
```

### 交互
```
用户用于交互的工具一般有三种：鼠标、键盘、触屏。
```
transition会影响元素的事件绑定
[event](https://github.com/d3/d3/blob/master/API.md#handling-events)

### 形状
[shapes](https://github.com/d3/d3-shape)
