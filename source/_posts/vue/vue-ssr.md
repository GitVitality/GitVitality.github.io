---
title: vue-ssr
date: 2019-02-23 10:40:51
tags: vue
---

为什么 vue-ssr？

1. 更好的 SEO，由于搜索引擎爬虫抓取工具可以直接查看完全渲染的页面。
2. 更快的内容到达时间 (time-to-content)。

服务器端渲染与预渲染
调研服务器端渲染是否有必要进行，否则可以针对特定的一些页面进行预渲染工作。

vue-server-renderer

```javascript
const renderer = createRenderer({
  template: require('fs').readFileSync('./index.template.html', 'utf-8'),
});
renderer.renderToString(app, (err, html) => {
  if (err) throw err;
  console.log(html);
  // => <div data-server-rendered="true">Hello World</div>
});
```

组件生命周期函数
beforeCreate、created(将副作用代码移动到 beforeMount 或 mounted 生命周期中)

避免状态单一，为每个请求创建一个新的根 Vue 实例
entry-client.js
entry-server.js

代码分割，异步组件

数据预取存储容器
在路由组件上暴露出一个自定义静态函数 asyncData，由于此函数会在组件实例化之前调用，所以它无法访问 this。需要将 store 和路由信息作为参数传递进去，在路由导航之前解析数据，需要添加 loading 状态，匹配要渲染的视图后，再获取数据。
