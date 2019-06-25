---
title: 解决开发网故障的问题
date: 2019-06-25 09:00:00
categories: basis
---

## 遇到问题

1. 敏捷开发时，后端定义完接口，但是无法和前端调试；
2. 前端开发过程中，开发网出现故障问题。

## 分析问题

1. 针对于问题一，可以使用 postman 定义接口并生成 mock-server;
2. 针对于问题二，则需要在开发网是正常的情况下，备份出请求正常的数据并生成 mock-server。

## 解决问题

1. 目前 postman 可以解决问题一，但是对于问题二来讲，postman 拦截请求后添加到 collection 没有对接口进行去重处理；
2. 需要手动保存 examples，mock-server 的速度非常缓慢。

## 优化工作

1. 能初始化 mock 数据；
2. 能截取网络请求，并更新 mock 数据，便于调试。

## 参考链接

[Fake it till you make it: mocks for agile development](https://medium.com/better-practices/https-medium-com-postman-engineering-fake-it-till-you-make-it-mocks-for-agile-development-f4d050cad694)
