---
title: vue-函数式编程应用
date: 2019-04-29 22:31:17
tags: vue
---

## 业务场景

在 element 的 Pagination 组件中，分别去监听 size-change 与 current-change 事件，同时又向后台发送数据请求。size-change 可能会引起 current-change，此时会发送两次请求，但是最终需要的结果是最后一次发送请求的结果。

## 解决方法

这种思想类似于函数防抖，函数防抖的思想是等待、等待、等待一段时间后，再去执行。

1. 利用 loadsh 函数库的 debounce 方法生成一个防抖函数去包装真实的后台数据请求；
2. 既然 loash 函数库可以，那么 rxjs 应该也可以。

### 方法二的坎坷旅途

1. 在探索 rxjs 的解决方法中耗费了一段时间，首先查看了 vue-rx 库，查看其 API。

- v-stream
- \$watchAsObservable(expOrFn, [options])
- \$eventToObservable(event)
- \$subscribeTo(observable, next, error, complete)
- \$fromDOMEvent(selector, event)
- \$createObservableMethod(methodName)

2. 在思索了一番后，发觉这些方法都不适合需求，接下来该继续向 rxjs 文档出发了。

rxjs 中有几个核心的概念

- Observables
- Subscription
- Subjects
- Operators

- Observables are lazy Push collections of multiple values.
- A Subscription is an object that represents a disposable resource, usually the execution of an Observable.
- An RxJS Subject is a special type of Observable that allows values to be multicasted to many Observers.
- Operators are functions.

可以将 Observables 当做是生产者，可以延迟生产值；将 Subscription 当做是消费者，可以订阅 Observables。

在这个业务需求中，两次后台请求则是生产值，而请求返回后将数据回填则是消费值。

最后采用了 Subject 对象，既可以生产值，也能够消费值。

```javascript
getList(params) {
  if (this.subject == null) {
    this.subject = new Subject()
    this.subject.pipe(debounceTime(100)).subscribe({
      next: val => {
        this.dispatchQuery(val)
      }
    })
  }
  this.subject.next(params)
}
```

当第一次调用 getList 方法时，生成 subject，并生产值；之后每次都会生产值；在消费者，则可以做一些过滤操作。
