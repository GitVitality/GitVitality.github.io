---
title: vue-cli
date: 2019-04-30 12:01:57
tags: [vue-cli]
categories: vue
---

## vue-cli 插件系统

最近看了 vue-cli 源码分析与 vue-cli 官方文档，学习了 vue-cli 基于插件的生态系统，同时也有一个较大的发现，vue-ui 也是基于插件的系统，vue-ui 项目中还使用了 apollo 的 graphql 技术。

## vue-ui 项目

最近在看 vue-ui 这个项目，首先是通过 vue-cli 项目过来的，vue-cli 与 vue-ui 项目都是通过插件机制进行扩展的，vue-cli 插件分为两部分，一是 service 插件，二是 generator。
service 插件是在 vue-cli-service 执行命令时进行调用的@vue-cli-service 的 pluginAPI，而 generator 是在项目构建和 add 和 invoke 时进行调用的，@vue-cli 的 GeneratorAPI。

vue-cli 包含了 vue-ui 项目，ui.js 是入口，pluginAPI 在@vue-cli-ui 中的 apollo-server api 中的 PluginAPI 文件中

apollo-server 中 server.js 中 connectors 中 plugins runPluginAPI

vue-ui 插件开发，需要开发 plugin ui.js 和 client addon (自定义组件)

client addon 可以通过 shared data 、plugin action 与 plugin 通信，而 webpack 不同的 plugin 则采用 IPC 通信。

vue-cli-plugin-apollo
service 插件注册了 apollo:watch 与 apollo:run 两个命令， apollo:watch 通过 nodemon 监听文件变动后重新运行 npm run apollo:run，如果有 —run 参数，则通过 runParallelCommand 函数，即 execa 库同步运行 npm run serve
