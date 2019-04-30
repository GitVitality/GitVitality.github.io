---
title: 浏览器缓存
date: 2019-02-23 09:00:00
tags: [basis, cache]
categories: basis
---

## 缓存控制

- HTTP/1.0 program
- HTTP/1.1 Cache-Control
  - no store、no cache
  - must-revalidate(强制缓存，但是需要确定缓存的新鲜度)
  - private、public
  - max-age=31536000(浏览器时间，与服务器时间不一致)

## 新鲜度

- If-Modified-Since(Last-Modified、服务器时间，但是只能精确到秒级)
- If-None-Match(Etag)

## 加速资源(revving 技术)

原理：尽量延长资源的缓存时间，但是无法通知资源是否更新，revving 使用添加版本号的方式在 html 文件中通知文件更新，添加了版本号的文件是一个新的独立资源请求。

## 参考文献

[HTTP 缓存](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Caching_FAQ)
