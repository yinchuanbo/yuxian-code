---
title: "前端图片压缩 js-image-compressor"
tag: "工具集"
time: 2024-09-01 15:21:24
---

### 前言

通常我们上传图片尤其是用于 web/移动端展示，需要考虑到图片的尺寸大小，前端可以使用一些工具对于较大图片进行压缩，尽量控制图片大小在 200kb 内，这样移动端在展示图片时较快加载，避免出现图片加载白屏问题。

对于这类问题，处理的思路有以下几种：

1. 上传图片时前端先进行图片压缩，压缩后传给 Server 或上传 CDN，移动端直接拿到图片不处理就可以展示。
2. 上传图片时前端不处理直接传给 Server，由 Server 处理，移动端展示。
3. 上传图片时前端和 Server 都不处理，移动端展示的时候先对图片预处理再展示。

首先第 3 种不推荐，在展示前进行压缩，除了某些业务场景需要保持原图质和灵活外可以用第 3 种；大部分情况下推荐第 1 种，在上传前由前端进行图片压缩再传给后端，这样可以**节省上传时**、**节省服务器存储空间**、**减少网络负担**。

### js-image-compressor 介绍

`js-image-compressor`  是一个 JavaScript 库，它允许你在客户端（浏览器）压缩图片。这对于上传图片到服务器时减少带宽使用或优化存储大小非常有用。

git 地址：[github.com/wuwhs/js-im…](https://github.com/wuwhs/js-image-compressor)

## 使用方式

1. 安装

```bash
npm install js-image-compressor
# 或
yarn add js-image-compressor
```

2. 使用

```js
import ImageCompressor from "js-image-compressor";

function imageCompress(file: any) {
  const size = file.size / 1024;
  return new Promise((resolve, reject) => {
    const options = {
      file: file,
      quality: 0.8, // 图片质量
      mimeType: "image/jpeg",
      maxWidth: file.height,
      maxHeight: file.width,
      minWidth: 10, // 指定压缩图片最小宽度
      width: 1080, // 指定压缩图片宽度
      convertSize: Infinity,
      loose: true,
      redressOrientation: true,
      success: (result) => {
        resolve(result);
      },
      error: (msg) => {
        reject(msg);
      },
    };
    new ImageCompressor(options);
  });
}
```

3. 对图片大小进行分层处理保证原图 20M 内的图片压缩后都控制在 200kb 内

```js
let quality = 1;
if (size < 200) {
  return file;
}
if (size > 200 && size <= 512) {
  quality = 0.9;
} else if (size > 512 && size <= 1024) {
  quality = 0.8;
} else if (size > 1024 && size <= 2048) {
  quality = 0.85;
} else if (size > 2048 && size <= 10240) {
  quality = 0.8;
} else if (size > 10240 && size <= 20480) {
  quality = 0.75;
}
```

### 总结

js-image-compressor 压缩图片就是改变图片的 quality 或者调整 size（width、height）；但是注意对于一些图片需要分情况处理，比如对于 size 较小的图如果固定 width 后可能会增大；最后，处理图片需要注意保证图片清晰度的情况下去压缩。
