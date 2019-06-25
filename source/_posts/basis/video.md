---
title: 前端视频处理
date: 2019-06-5 09:00:00
categories: basis
---

## 遇到问题

1. 视频上传，nginx 上传文件大小设置；
2. 视频缩略图截取，采用 canvas 的形式，但是部分视频格式不支持，提示用户；
3. 视频播放，拖动以及 safari 浏览器播放需要 http 响应头部支持。

## 分析问题

1. 视频转码，ffmpeg，相应的前端库有 ffmpeg.js、videoconvert.js，采用 emcc 将 ffmepg 编译成 asmscript 或者 webassembly；
2. 视频实时流媒体处理，webrtc、getusermedia、 media source；
3. http 206。

## 解决问题

[wasm + ffmpeg 实现前端截取视频帧功能](https://www.yinchengli.com/2018/07/28/wasm-ffmpeg-get-video-frame/)，编译过程中遇到的问题。

- 编译时无法找到头文件，需要用 pkg-config 添加头文件；
- 编译出错，emscripten 安装的版本不对；
- 初始化 webassembly 出错, RangeError。

## 参考链接

[ffmpeg.js](https://github.com/Kagami/ffmpeg.js/)
[videoconverter.js](https://github.com/bgrins/videoconverter.js)
[ffmpeg-wasm-video-to-picture](https://github.com/liyincheng/ffmpeg-wasm-video-to-picture)
[wasm + ffmpeg 实现前端截取视频帧功能](https://www.yinchengli.com/2018/07/28/wasm-ffmpeg-get-video-frame/)
