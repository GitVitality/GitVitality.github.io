---
title: webpack
date: 2019-02-23 10:40:51
tags: webpack
---

[深入浅出 Webpack](http://webpack.wuhaolin.cn/)

构建工具的作用
代码转换：TypeScript 编译成 JavaScript、SCSS 编译成 CSS 等。
文件优化：压缩 JavaScript、CSS、HTML 代码，压缩合并图片等。
代码分割：提取多个页面的公共代码、提取首屏不需要执行部分的代码让其异步加载。
模块合并：在采用模块化的项目里会有很多个模块和文件，需要构建功能把模块分类合并成一个文件。
自动刷新：监听本地源代码的变化，自动重新构建、刷新浏览器。
代码校验：在代码被提交到仓库前需要校验代码是否符合规范，以及单元测试是否通过。
自动发布：更新完代码后，自动构建出线上发布代码并传输给发布系统。

Npm Script
Grunt
Gulp
Fis3
Webpack
Webpack 是一个打包模块化 JavaScript 的工具，在 Webpack 里一切文件皆模块，通过 Loader 转换文件，通过 Plugin 注入钩子，最后输出由多个模块组合成的文件。Webpack 专注于构建模块化项目。

1. 多入口配置，相对于配置文件的相对路径 path.resolve(\_\_dirname)
2. 常用插件

- ExtractTextPlugin，提取 css 文件
- SplitChunksPlugin，合并代码
- SpritesmithPlugin，自动生成精灵图的插件

3. devServer，实时预览(监听模式)、模块热替换、支持 Source Map
   devServer 与浏览器采用 webSocket 协议进行通信，当文件发生变化后，devServer 会通知浏览器

Entry：入口，Webpack 执行构建的第一步将从 Entry 开始，可抽象成输入。
Module：模块，在 Webpack 里一切皆模块，一个模块对应着一个文件。Webpack 会从配置的 Entry 开始递归找出所有依赖的模块。
Chunk：代码块，一个 Chunk 由多个模块组合而成，用于代码合并与分割。
Loader：模块转换器，用于把模块原内容按照需求转换成新内容。
Plugin：扩展插件，在 Webpack 构建流程中的特定时机注入扩展逻辑来改变构建结果或做你想要的事情。
Output：输出结果，在 Webpack 经过一系列处理并得出最终想要的代码后输出结果。

写过一个插件、vscode 插件

<!-- 需要加强 -->
