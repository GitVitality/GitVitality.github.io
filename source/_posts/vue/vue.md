---
title: vue框架
date: 2019-04-26 09:00:00
tags: [vue]
categories: vue
---

## mvc、mvp、mvvm

- smalltalk mvc
  用控制器响应 view(用户)输入、调用 model 接口对数据进行操作，一旦 model 发生变化，通知相关视图进行更新。model-view 观察者模式，model 中存储了相关的视图，一旦数据发生了变化，通知所有的视图进行更新。view-controller 策略者模式，如果要实现不同的响应策略，只需要引用不同的 controller 即可。

  缺点：view 已经具备了独立处理用户事件的能力，全部写在 controller 里面会导致 controller 十分臃肿，view 与 controller 一般是一一对应的，controller 的复用性不好。

- taligent mvp
  在 mvc 里，view 可以直接访问 model，mvp 切断了 view 与 model 的联系，让 presenter 更新 model，再通过观察者模式通知 view。用户对 view 的操作都转移到了 presenter。

  缺点：presenter 作为中间人，除了维护基本的业务逻辑，还要对从 view 到 model 的数据和 model 到 view 的数据进行同步。presenter 如果维护过多的视图需求，将会出现较多的改动。

- 微软 mvvm
  和 mvc/mvp 不同的是，mvvm 中的 view 通过使用模板语法来声明式的将数据渲染进 DOM，当 viewmodel 对 model 进行更新的时候，会通过数据绑定更新到 view。

  优点：不仅仅简化了业务与界面的依赖，还解决了数据频繁更新（以前用 jQuery 操作 DOM 很繁琐）的问题。

## 双向数据绑定

### 数据绑定方式

- 数据劫持 (vue)
- 发布-订阅模式 (knockout、backbone)
- 脏值检查 (angular)

在 Vue 中，使用了双向绑定技术（two-way-data-binding），就是 view 的变化能实时让 model 发生变化，而 model 的变化也能实时更新到 view。Vue 采用数据劫持&发布-订阅模式的方式。

## 组件生命周期

- beforeCreate->created->beforeMount->mounted->beforeDestroy->destroyed
- beforeUpdate->updated
- activated-deactivated

可以看到，在初始化流程、 update 流程和销毁流程中，子级的相应声明周期方法都是在父级相应周期方法之间调用的。比如子级的初始化钩子函数（ beforeCreate 、 created 、 mounted ）都是在父级的 created 和 mounted 之间调用的，这实际上说明等到子级准备好了，父级才会将自己挂载到上一层 DOM 树中去，从而保证界面上不会闪现脏数据。

## 指令

```javascript
Vue.directive('my-directive', {
  bind() {},
  inserted() {},
  update() {},
  componentUpdated() {},
  unbind() {},
});
```

## 路由

### 全局守卫

- beforeEach
- afterEach
- beforeEnter

### 组件内的守卫

- beforeRouteEnter
- beforeRouteUpdate
- beforeRouteLeave

### 路由懒加载

vue 异步组件和 webpack 的代码分割功能。

#### vue 异步组件，返回一个 Promise 的工厂函数

```javascript
const Foo = () => Promise.resolve({ /_ 组件定义对象 _/ })
```

#### 动态的 import 语法定义代码分割点

```javascript
import('./Foo.vue'); // 返回 Promise
```

babel 需要 syntax-dynamic-import 插件的支持，结合这两者，这就是如何定义一个能够被 Webpack 自动代码分割的异步组件。

```javascript
const Foo = () => import('./Foo.vue');
```

#### 使用命名 chunk 将组件进行按组分块

```javascript
const Foo = () => import(/_ webpackChunkName: "group-foo" _/ './Foo.vue')
const Bar = () => import(/_ webpackChunkName: "group-foo" _/ './Bar.vue')
const Baz = () => import(/_ webpackChunkName: "group-foo" _/ './Baz.vue')
```

## vuex

- state
- getter
- mutation
- action
- module

核心理念：将所有的异步操作最后汇总为 mutation 操作去修改 store 的状态。

## 虚拟 dom

虚拟 dom 本质是因为 dom 操作性能不好，所以采用一个 js 对象表示 dom 树，直接在对象上进行操作，然后再差异化更新，浏览器重新渲染。虚拟 dom 算法本质是表示 dom 树的两个 js 对象的对比，树形结构。

create->diff->patch 过程。

## vue 组件设计

### 基础复用（单个组件）

- props
- events
- 插槽

### 高级复用（组件间）

- mixins
- directives
- 渲染函数
- 插件
- 过滤器

## 参考文献

[浅析前端开发中的 mvc/mvp/mvvm 模式](https://juejin.im/post/593021272f301e0058273468)
[发布-订阅模式 和 观察者模式区别](https://juejin.im/post/5a14e9edf265da4312808d86)
[从头开始学习 vue-router](https://juejin.im/post/5b0281b851882542845257e7)
[Vuex 框架原理与源码分析](https://juejin.im/entry/5a4b504b51882527a13ddcff)
