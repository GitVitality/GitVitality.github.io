---
title: vue-ssr
date: 2019-04-27 09:00:00
tags: [vue, vue-ssr]
categories: vue
---

## 原因

- 更好的 SEO，由于搜索引擎爬虫抓取工具可以直接查看完全渲染的页面。
- 更快的内容到达时间 (time-to-content)。

## vue-server-renderer

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
