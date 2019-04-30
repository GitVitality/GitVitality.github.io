---
title: html2canvas
date: 2019-02-25 09:00:00
tags: [lib]
categories: lib
---

## 业务场景

采用 html2canvas 库将图片转成 canvas，再通过 jspdf 库打印图片。

## 初次解决方案

设置 html2canvas 库的参数 allowTaint、useCORS；由于请求了跨域资源引起了画布污染、此时可以完整的将 dom 节点转成 canvas；当将 canvas 采用 toDataURL()时，需要获取图片的资源，此时却没有获取到图片的资源。

## 失败原因

虽然传递了 useCORS 参数，去请求百度地图跨域的图片资源，但是由于第一次渲染时，没有添加跨域请求头，等到第二次需要获取资源时再添加 origin 参数，会直接从缓存读取图片内容，chrome 对此做出安全限制。

## 最终解决方案

- 在第二次获取资源时，添加时间戳作为查询参数，重新向服务器请求资源。（需要修改源码）
- 不使用 https 请求，chrome 浏览器对 http 请求没有做安全控制。
- 禁用浏览器缓存。（对用户不友好）

crossOrigin: Anonymous

## 参考文献

[HTTP 访问控制（CORS）](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)
