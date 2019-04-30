---
title: vue框架
date: 2019-02-23 10:40:51
tags: vue
---

1. mvc、mvp、mvvm
   [浅析前端开发中的 MVC/MVP/MVVM 模式](https://juejin.im/post/593021272f301e0058273468)

- 模型到视图的映射

```javascript
var myapp = {}; // 创建这个应用对象

myapp.Model = function() {
  var val = 0; // 需要操作的数据

  /* 操作数据的方法 */
  this.add = function(v) {
    if (val < 100) val += v;
  };

  this.sub = function(v) {
    if (val > 0) val -= v;
  };

  this.getVal = function() {
    return val;
  };
};

myapp.View = function() {
  /* 视图元素 */
  var $num = $('#num'),
    $incBtn = $('#increase'),
    $decBtn = $('#decrease');

  /* 渲染数据 */
  this.render = function(model) {
    $num.text(model.getVal() + 'rmb');
  };
};
```

- Smalltalk MVC
  用控制器响应 view(用户)输入、调用 model 接口对数据进行操作，一旦 model 发生变化，通知相关视图进行更新。
  model-view 观察者模式，model 中存储了相关的视图，一旦数据发生了变化，通知所有的视图进行更新。
  view-controller 策略者模式，如果要实现不同的响应策略，只需要引用不同的 controller 即可。

缺点：view 已经具备了独立处理用户事件的能力，全部写在 controller 里面会导致 controller 十分臃肿，view 与 controller 一般是一一对应的，controller 的复用性不好。

- Taligent MVP
  在 mvc 里，view 可以直接访问 model，mvp 切断了 view 与 model 的联系，让 Presenter 更新 model，再通过观察者模式通知 view。用户对 view 的操作都转移到了 Presenter。

  Presenter 作为中间人，除了维护基本的业务逻辑，还要对从 view 到 model 的数据和 model 到 view 的数据进行同步。Presenter 如果维护过多的视图需求，将会出现较多的改动。

- 微软 MVVM
  和 MVC/MVP 不同的是，MVVM 中的 View 通过使用模板语法来声明式的将数据渲染进 DOM，当 ViewModel 对 Model 进行更新的时候，会通过数据绑定更新到 View。

  不仅仅简化了业务与界面的依赖，还解决了数据频繁更新（以前用 jQuery 操作 DOM 很繁琐）的问题。

2. 双向数据绑定
   在 Vue 中，使用了双向绑定技术（Two-Way-Data-Binding），就是 View 的变化能实时让 Model 发生变化，而 Model 的变化也能实时更新到 View。

数据绑定的方式

- 数据劫持 (Vue)
- 发布-订阅模式 (Knockout、Backbone)
- 脏值检查 (Angular)

Vue 采用数据劫持&发布-订阅模式的方式

[发布-订阅模式 和 观察者模式区别](https://juejin.im/post/5a14e9edf265da4312808d86)

- 观察者模式 在软件设计中是一个对象，维护一个依赖列表，当任何状态发生改变自动通知它们。
- 发布-订阅模式 中间层，发布者不直接调用订阅者，中间有一些处理过程，也有可能存在异步操作。

3. 组件生命周期
   beforeCreate->created->beforeMount->mounted->beforeDestroy->destroyed

   beforeUpdate->updated

   activated-deactivated

   同时，可以看到，在初始化流程、 update 流程和销毁流程中，子级的相应声明周期方法都是在父级相应周期方法之间调用的。比如子级的初始化钩子函数（ beforeCreate 、 created 、 mounted ）都是在父级的 created 和 mounted 之间调用的，这实际上说明等到子级准备好了，父级才会将自己挂载到上一层 DOM 树中去，从而保证界面上不会闪现脏数据。

4. 指令生命周期

   ```javascript
   Vue.directive('mydirective', {
     bind() {},
     inserted() {},
     update() {},
     componentUpdated() {},
     unbind() {},
   });
   ```

5. vue-router
   [从头开始学习 vue-router](https://juejin.im/post/5b0281b851882542845257e7)
   单页面应用(SPA)的核心之一是: 更新视图而不重新请求页面

   全局前置守卫
   beforeEach
   全局后置钩子
   afterEach
   路由独享守卫
   beforeEnter
   组件内的守卫
   beforeRouteEnter
   beforeRouteUpdate
   beforeRouteLeave

   路由懒加载
   vue 异步组件和 webpack 的代码分割功能

   vue 异步组件，返回一个 Promise 的工厂函数
   const Foo = () => Promise.resolve({ /_ 组件定义对象 _/ })

   动态的 import 语法定义代码分割点
   import('./Foo.vue') // 返回 Promise

   babel 需要 syntax-dynamic-import 插件的支持，结合这两者，这就是如何定义一个能够被 Webpack 自动代码分割的异步组件。
   const Foo = () => import('./Foo.vue')

   使用命名 chunk 将组件进行按组分块
   const Foo = () => import(/_ webpackChunkName: "group-foo" _/ './Foo.vue')
   const Bar = () => import(/_ webpackChunkName: "group-foo" _/ './Bar.vue')
   const Baz = () => import(/_ webpackChunkName: "group-foo" _/ './Baz.vue')

6. vuex
   [Vuex 框架原理与源码分析](https://juejin.im/entry/5a4b504b51882527a13ddcff)

   state
   getter
   mutation
   action
   module

   将所有的异步操作最后汇总为 mutation 操作去修改 store 的状态

7. 虚拟 dom
   虚拟 dom 本质是因为 dom 操作性能不好，所以采用一个 js 对象表示 dom 树，直接在对象上进行操作，然后再差异化更新，浏览器重新渲染。
   虚拟 dom 算法本质是表示 dom 树的两个 js 对象的对比，树形结构。

   create->diff->patch 过程

   <!-- 需要加强 -->

8. vue 组件设计

   - 基础复用（单个组件）
   - props
   - events
   - 插槽

   - 高级复用（组件间）
   - mixins
   - directives
   - 渲染函数
   - 插件
   - 过滤器

9. 异步组件三种方式

- 普通异步组件（function 形式）
- promise（promise 形式）
- 高级异步组件（四种状态，可以分别添加组件 loading、resolve、reject、timeout ）

异步组件实现的本质是 2 次渲染，除了 0 delay 的高级异步组件第一次直接渲染成 loading 组件外，其它都是第一次渲染生成一个注释节点，当异步获取组件成功后，再通过 forceRender 强制重新渲染，这样就能正确渲染出我们异步加载的组件了。

其实前端开发最重要的 2 个工作，一个是把数据渲染到页面，另一个是处理用户交互。

从 vue.js 技术揭秘中学到的

准备工作

1. 从 Vue.js 的目录设计可以看到，作者把功能模块拆分的非常清楚，相关的逻辑放在一个独立的目录下维护，并且把复用的代码也抽成一个独立目录。这样的目录设计让代码的阅读性和可维护性都变强，是非常值得学习和推敲的。
2. 这一节的目的是让同学们对 Vue 是什么有一个直观的认识，它本质上就是一个用 Function 实现的 Class，然后它的原型 prototype 以及它本身都扩展了一系列的方法和属性，那么 Vue 能做什么，它是怎么做的，我们会在后面的章节一层层帮大家揭开 Vue 的神秘面纱。

数据驱动

1. Vue 初始化主要就干了几件事情，合并配置，初始化生命周期，初始化事件中心，初始化渲染，初始化 data、props、computed、watcher 等等。
2. [函数柯里化技巧](https://ustbhuangyi.github.io/vue-analysis/data-driven/update.html#%E6%80%BB%E7%BB%93)
3. [深度优先遍历，递归版 normalizeArrayChildren](https://ustbhuangyi.github.io/vue-analysis/data-driven/create-element.html#children-%E7%9A%84%E8%A7%84%E8%8C%83%E5%8C%96)
4. [Vue \_update](https://ustbhuangyi.github.io/vue-analysis/data-driven/update.html#%E6%80%BB%E7%BB%93)
5. {{{ new Vue() -> init -> $mount -> compile -> render -> vnode -> patch -> dom }}}

- 柯里化函数
- 深度优先遍历
- 初始化过程
- 继承关系
- 组件钩子函数的调用关系

组件化

1. Vue.extend 的作用就是构造一个 Vue 的子类，它使用一种非常经典的原型继承的方式把一个纯对象转换一个继承于 Vue 的构造器 Sub 并返回，然后对 Sub 这个对象本身扩展了一些属性，如扩展 options、添加全局 API 等；并且对配置中的 props 和 computed 做了初始化工作；最后对于这个 Sub 构造函数做了缓存，避免多次执行 Vue.extend 的时候对同一个子组件重复构造。
2. 这一节我们分析了 createComponent 的实现，了解到它在渲染一个组件的时候的 3 个关键逻辑：构造子类构造函数，安装组件钩子函数和实例化 vnode。
3. 我们详细地介绍了 Vue.js 合并 options 的过程，各个阶段的生命周期的函数也被合并到 vm.\$options 里，并且是一个数组。因此 callhook 函数的功能就是调用某个生命周期钩子注册的所有回调函数。
4. 我们可以看到，每个子组件都是在这个钩子函数中执行 mouted 钩子函数，并且我们之前分析过，insertedVnodeQueue 的添加顺序是先子后父，所以对于同步渲染的子组件而言，mounted 钩子函数的执行顺序也是先子后父。
5. 在 \$destroy 的执行过程中，它又会执行 vm.**patch**(vm.\_vnode, null) 触发它子组件的销毁钩子函数，这样一层层的递归调用，所以 destroy 钩子函数执行顺序是先子后父，和 mounted 过程一样。

深入响应式原理

初始化过程

```javascript
export function initState(vm: Component) {
  vm._watchers = [];
  const opts = vm.$options;
  if (opts.props) initProps(vm, opts.props);
  if (opts.methods) initMethods(vm, opts.methods);
  if (opts.data) {
    initData(vm);
  } else {
    observe((vm._data = {}), true /* asRootData */);
  }
  if (opts.computed) initComputed(vm, opts.computed);
  if (opts.watch && opts.watch !== nativeWatch) {
    initWatch(vm, opts.watch);
  }
}
```

代理的实现

```javascript
const sharedPropertyDefinition = {
  enumerable: true,
  configurable: true,
  get: noop,
  set: noop,
};

export function proxy(target: Object, sourceKey: string, key: string) {
  sharedPropertyDefinition.get = function proxyGetter() {
    return this[sourceKey][key];
  };
  sharedPropertyDefinition.set = function proxySetter(val) {
    this[sourceKey][key] = val;
  };
  Object.defineProperty(target, key, sharedPropertyDefinition);
}
```

Observer

依赖收集，在 getter 的时候 dep 收集 watcher 依赖
派发更新，在 setter 的时候 dep 通知 watcher，watcher 再调用 update 函数

这里引入了一个队列的概念，这也是 Vue 在做派发更新的时候的一个优化的点，它并不会每次数据改变都触发 watcher 的回调，而是把这些 watcher 先添加到一个队列里，然后在 nextTick 后执行 flushSchedulerQueue。

nextTick(flushSchedulerQueue)

nextTick 实现原理

组件更新
节点相同、节点不同（删除、新建）、静态节点优化

编译
模板解析，生成 AST
那么至此，parse 的过程就分析完了，看似复杂，但我们可以抛开细节理清它的整体流程。parse 的目标是把 template 模板字符串转换成 AST 树，它是一种用 JavaScript 对象的形式来描述整个模板。那么整个 parse 的过程是利用正则表达式顺序解析模板，当解析到开始标签、闭合标签、文本的时候都会分别执行对应的回调函数，来达到构造 AST 树的目的。

AST 元素节点总共有 3 种类型，type 为 1 表示是普通元素，为 2 表示是表达式，为 3 表示是纯文本。

优化： 标记静态节点、标记静态根

codegen（将优化后的 AST 代码转化成可执行性的代码）

事件
v-model
slot 普通插槽、作用域插槽
keep-alive
那么至此，<keep-alive> 的实现原理就介绍完了，通过分析我们知道了 <keep-alive> 组件是一个抽象组件，它的实现通过自定义 render 函数并且利用了插槽，并且知道了 <keep-alive> 缓存 vnode，了解组件包裹的子元素——也就是插槽是如何做更新的。且在 patch 过程中对于已缓存的组件不会执行 mounted，所以不会有一般的组件的生命周期函数但是又提供了 activated 和 deactivated 钩子函数。另外我们还知道了 <keep-alive> 的 props 除了 include 和 exclude 还有文档中没有提到的 max，它能控制我们缓存的个数。
transition
Vue.js 除了实现了强大的数据驱动，组件化的能力，也给我们提供了一整套过渡的解决方案。它内置了 <transition> 组件，我们可以利用它配合一些 CSS3 样式很方便地实现过渡动画，也可以利用它配合 JavaScript 的钩子函数实现过渡动画，在下列情形中，可以给任何元素和组件添加 entering/leaving 过渡：

vue-router
vue 插件机制
install 函数
路径切换
