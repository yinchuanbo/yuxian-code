---
title: "速通 JS 性能优化"
tag: "性能优化"
time: 2024-08-30 17:34:29
---

### 1. 内存管理方案

**1.1 全局变量导致内存泄漏问题**

使用全局变量可能会导致内存泄漏，因为它们在程序终止之前不会自动被垃圾回收。

```js
// 具有全局变量的内存泄漏示例
let globalArray = [];
function addToGlobalArray(item) {
  globalArray.push(item);
}

// 使用局部变量修复这个问题
function manageArray() {
  let localArray = [];
  function addToArray(item) {
    localArray.push(item);
  }
}
```

**1.2 闭包引发的内存泄漏**

即使外部函数已经返回，闭包仍可以保留对变量的引用，如果使用不当，可能会导致内存泄漏

```js
// 闭包导致内存泄漏的场景
function outerFunction() {
  let largeArray = new Array(1000000).fill("data");
  return function innerFunction() {
    console.log(largeArray.length);
  };
}
const inner = outerFunction();
inner();

// 通过显示的清除方案，解决内存泄漏的问题
function outerFunction() {
  let largeArray = new Array(1000000).fill("data");
  return function innerFunction() {
    console.log(largeArray.length);
    largeArray = null; // 显示清除
  };
}
const inner = outerFunction();
inner();
```

**1.3 事件监听导致内存泄漏问题**

当不再需要事件监听器时未能删除它们可能会导致内存泄漏

```js
// 事件监听器的可能会导致内存泄漏
function addEventListenerExample() {
  document.querySelector("button").addEventListener("click", function () {
    console.log("Button clicked");
  });
}

// 手动清除监听器
function addEventListenerExample() {
  const button = document.querySelector("button");
  const clickHandler = function () {
    console.log("Button clicked");
  };
  button.addEventListener("click", clickHandler); // 删除监听
  button.removeEventListener("click", clickHandler);
}
```

**1.4 DOM 节点导致的内存泄漏**

引用的已经删除的 DOM 节点可能会导致内存泄漏。当删除节点时，需要清理 DOM 引用

```js
// DON 节点的引用
let element = document.createElement("div");
document.body.appendChild(element);
document.body.removeChild(element); // DOM 节点被删除
// 手动清理引用
element = null;
```

### 2. 基于 Web Worker 进行性能提升

### 3. 数据结构提升 JS 性能

选择正确的数据结构会显著影响 JavaScript 应用程序的性能。高效的数据结构可以提高搜索、排序和操作数据等操作的速度和内存使用率

```js
// 基于 Set 构建唯一值的集合
const uniqueValues = new Set([1, 2, 3, 4, 5, 5, 6]);
uniqueValues.add(7);
uniqueValues.delete(3);
console.log(uniqueValues.has(2)); // true
console.log(uniqueValues.size); // 6
// 基于 Map 构建 key-value 结构
const map = new Map();
map.set("key1", "value1");
map.set("key2", "value2");
console.log(map.get("key1")); // 'value1'
map.delete("key2");
console.log(map.size); // 1
```

### 4. 基于 WebAssembly 处理密集任务

WebAssembly (Wasm) 是一种二进制指令格式（文档：https://developer.mozilla.org/zh-CN/docs/WebAssembly/Concepts），可在 Web 上实现代码的高性能执行。它允许开发人员使用 C、C++ 和 Rust 等语言编写性能关键型代码，并与 JavaScript 一起运行。

```rs
// src/lib.rs
#[wasm_bindgen]
pub fn fibonacci(n: u32) -> u32 {
    match n {
        0 => 0,
        1 => 1,
        _ => fibonacci(n - 1) + fibonacci(n - 2),
    }
}
```

```js
//加载和执行 WebAssembly 模块的 js 代码
fetch("fibonacci.wasm")
  .then((response) => response.arrayBuffer())
  .then((bytes) => WebAssembly.instantiate(bytes))
  .then((result) => {
    const fibonacci = result.instance.exports.fibonacci;
    console.log(fibonacci(10)); // Output: 55
  });
```
