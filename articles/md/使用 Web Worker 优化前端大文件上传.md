---
title: "使用 Web Worker 优化前端大文件上传"
tag: "Web Worker"
time: 2024-09-01 15:21:24
---

### 1\. 分析任务

上传大文件时，为防止长时间占用主线程，可以将文件分割成多个小片段（chunks），每个片段独立上传。利用 Web Workers 可以并行处理文件的读取、分片、上传等任务。

### 2\. 设置 Web Worker 环境

首先，你需要创建一个 Worker 脚本。例如，`uploadWorker.js`，该脚本将用来处理文件的读取及上传逻辑。

```js
// uploadWorker.js
self.onmessage = function (e) {
  const { fileChunk, uploadUrl } = e.data;
  // 处理文件上传逻辑
  uploadFileChunk(fileChunk, uploadUrl);
};
function uploadFileChunk(chunk, url) {
  // 使用Fetch API或XMLHttpRequest上传文件片段
  fetch(url, {
    method: "POST",
    body: chunk,
  })
    .then((response) => {
      return response.json();
    })
    .then((data) => {
      self.postMessage({ status: "uploaded", data: data });
    })
    .catch((error) => {
      self.postMessage({ status: "error", error: error });
    });
}
```

## 3\. 在主线程中使用 Web Worker

在主线程，我们创建 Worker，并处理文件分割，然后发送文件块到 Worker。

```js
// 主文件 main.js
const worker = new Worker("uploadWorker.js");
worker.onmessage = function (e) {
  console.log("从Worker返回的消息:", e.data);
};
function handleFileUpload(file) {
  const chunkSize = 1024 * 1024 * 5; // 分割大小，例如5MB
  let currentChunk = 0;
  const chunks = [];
  while (currentChunk < file.size) {
    chunks.push(
      file.slice(currentChunk, Math.min(file.size, currentChunk + chunkSize))
    );
    currentChunk += chunkSize;
  }
  chunks.forEach((chunk) => {
    // 将每个chunk发送至worker处理
    worker.postMessage({
      fileChunk: chunk,
      uploadUrl: "https://example.com/upload",
    });
  });
}
// 监听文件选择器的改变
document
  .querySelector("#fileInput")
  .addEventListener("change", function (event) {
    const file = event.target.files[0];
    handleFileUpload(file);
  });
```
