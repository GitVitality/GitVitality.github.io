---
title: 浏览器缓存、客户端存储、web worker api
date: 2019-02-23 10:40:51
tags: basis
---

## 浏览器缓存

[HTTP 缓存](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Caching_FAQ)

1. 缓存类型分为私有浏览器缓存和共享代理缓存
2. 缓存操作的目标是 GET 响应
3. 缓存控制

- HTTP/1.0 program(向后兼容)
- HTTP/1.1 Cache-Control
  - no store、no cache
  - must-revalidate(强制缓存，但是需要确定缓存的新鲜度)
  - private、public
  - max-age=31536000(浏览器时间，与服务器时间不一致)

4. 新鲜度

- If-Modified-Since(Last-Modified、服务器时间，但是只能精确到秒级)
- If-None-Match(Etag)
- expirationTime = responseTime + freshnessLifetime - currentAge(缓存失效的时间计算)

5. 加速资源(revving 技术)

- 原理：尽量延长资源的缓存时间，但是无法通知资源是否更新，revving 使用添加版本号的方式在 html 文件中通知文件更新，添加了版本号的文件是一个新的独立资源请求。

## 客户端存储

- cookie、session(cookie 存储在客户端、session 存储在服务器，验证时生成一个令牌给客户端存储，在每次请求时带上令牌，http-only)
- sessionStorage、localStorage(浏览器的会话窗口时间、永久性存储)
- indexedDB(异步数据库)

## web worker api

- 百度地图热力图涉及到大量节点需要重新计算经纬度，页面上有很多的绘制元素，importScripts，postMessage、onmessage
- [开源库](http://lbsyun.baidu.com/index.php?title=jspopular/openlibrary)
- 绘制大量节点时，会有性能消耗(mapv)
