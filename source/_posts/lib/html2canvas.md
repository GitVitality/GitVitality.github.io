---
title: html2canvas
date: 2019-02-23 10:40:51
tags: lib
---

1. 设置 html2canvas 库的参数 allowTaint、useCORS
2. 由于请求了跨域资源引起了画布污染、此时可以完整的将 dom 节点转成 canvas
3. 当将 canvas 采用 toDataURL()时，需要获取图片的资源，此时却没有获取到图片的资源

原因：虽然传递了 useCORS 参数，去请求百度地图跨域的图片资源，但是由于第一次渲染时，没有添加跨域请求头，等到第二次需要获取资源时再添加 origin 参数，会直接从缓存读取图片内容，chrome 对此做出安全限制。

解决方案：在第二次获取资源时，添加时间戳作为查询参数，重新向服务器请求资源

crossOrigin: Anonymous
请求头部
Origin: http://foo.example
Access-Control-Request-Method: POST
Access-Control-Request-Headers: X-PINGOTHER, Content-Type

响应头部
Access-Control-Allow-Origin: http://foo.example
Access-Control-Allow-Methods: POST, GET, OPTIONS
Access-Control-Allow-Headers: X-PINGOTHER, Content-Type

跨域：来源、方法

[HTTP 访问控制（CORS）](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)

预检请求

递归取出目标模版的所有 DOM 节点，填充到一个 rederList，并附加是否为顶层元素/包含内容的容器 等信息

通过 z-index postion float 等 css 属性和元素的层级信息将 rederList 排序，计算出一个 canvas 的 renderQueue

遍历 renderQueue，将 css 样式转为 setFillStyle 可识别的参数，依据 nodeType 调用相对应 canvas 方法，如文本则调用 fillText，图片 drawImage，设置背景色的 div 调用 fillRect 等

将画好的 canvas 填充进页面
