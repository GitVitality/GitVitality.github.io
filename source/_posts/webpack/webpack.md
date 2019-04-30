---
title: webpack
date: 2019-02-28 09:00:00
tags: [webpack]
categories: webpack
---

## 打包工具的历程

- Npm Script
- Grunt
- Gulp
- Fis3
- webpack

## webpack 作用

- 代码转换：TypeScript 编译成 JavaScript、SCSS 编译成 CSS 等。
- 文件优化：压缩 JavaScript、CSS、HTML 代码，压缩合并图片等。
- 代码分割：提取多个页面的公共代码、提取首屏不需要执行部分的代码让其异步加载。
- 模块合并：在采用模块化的项目里会有很多个模块和文件，需要构建功能把模块分类合并成一个文件。
- 自动刷新：监听本地源代码的变化，自动重新构建、刷新浏览器。
- 代码校验：在代码被提交到仓库前需要校验代码是否符合规范，以及单元测试是否通过。
- 自动发布：更新完代码后，自动构建出线上发布代码并传输给发布系统。

webpack 是一个打包模块化 JavaScript 的工具，在 webpack 里一切文件皆模块，通过 Loader 转换文件，通过 Plugin 注入钩子，最后输出由多个模块组合成的文件。webpack 专注于构建模块化项目。

## webpack

- Entry：入口，webpack 执行构建的第一步将从 Entry 开始，可抽象成输入。
- Module：模块，在 webpack 里一切皆模块，一个模块对应着一个文件。webpack 会从配置的 Entry 开始递归找出所有依赖的模块。
- Chunk：代码块，一个 Chunk 由多个模块组合而成，用于代码合并与分割。
- Loader：模块转换器，用于把模块原内容按照需求转换成新内容。
- Plugin：扩展插件，在 webpack 构建流程中的特定时机注入扩展逻辑来改变构建结果或做你想要的事情。
- Output：输出结果，在 webpack 经过一系列处理并得出最终想要的代码后输出结果。

## webpack 小实践

### 写一个处理 vue 文件插件

### 使用 SpritesmithPlugin，自动生成精灵图

## 参考文献

[深入浅出 webpack](http://webpack.wuhaolin.cn/)
