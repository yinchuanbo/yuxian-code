---
title: "document.referrer 拦截问题"
tag: "JavaScript"
time: 2024-09-01 15:21:24
---

## 问题描述

- 最近遇到一个有意思的问题，项目中有一个地方，点击需要跳转到一个新的域名地址
- 笔者使用 a 标签做跳转，跳是跳过去了，可是跳过去以后，反而打不开了，显示 403

大致这样的代码：

```html
<a href="http://abcdefg.com" target="_blank">点击跳转</a>
```

## 原因分析

既然跳过去出问题，那么猜测是另外一个项目做了拦截

于是就去问问之前负责过`http://abcdefg.com`这个项目的同事

被告知：

为了安全考虑，对 [document.referrer](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/referrer) 进行了拦截判断（前后端均可拦截操作）

[developer.mozilla.org/zh-CN/docs/…](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/referrer) 了解，既然直接跳过去，会把 referrer 带着，那么就想办法，不带着就行了

## 5 种解决方案

**from**

_推荐下面的解决方案三_

```html
<!DOCTYPE html>

<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>from</title>
    <!-- 解决方案一 禁内容referrer -->
    <!-- <meta name="referrer" content="never"> -->
    <!-- 解决方案二 不带着referrer -->
    <!-- <meta name="referrer" content="no-referrer"> -->
  </head>
  <body>
    <!-- 解决方案三 a标签加rel属性控制 -->
    <a
      href="http://127.0.0.1:5502/referrer.html"
      target="_blank"
      rel="noopener noreferrer"
      >点击跳转</a
    >

    <!-- 解决方案四 使用referrerpolicy属性控制 -->
    <!-- <a href="http://127.0.0.1:5500/referrer.html" target="_blank" referrerpolicy="no-referrer">点击跳转</a> -->
    <!-- 解决方案五 换成window.open并注入js执行代码 -->
    <!-- <button>点击跳转</button>
 <script>
 let btn = document.querySelector('button')
 btn.onclick = () => {
 window.open('javascript:window.name;', `
 <script>location.replace("http://127.0.0.1:5502/referrer.html")<\/script>
 `)
 }
 </script> -->
  </body>
</html>
```

**referrer**

```html
<!DOCTYPE html>

<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>referrer</title>
  </head>
  <body>
    <h2></h2>
    <script>
      let referrer = document.referrer;
      let h2 = document.querySelector("h2");
      if (referrer) {
        h2.innerHTML = "不允许从别的地方跳转过来访问";
      } else {
        h2.innerHTML = "欢迎直接访问";
      }
    </script>
  </body>
</html>
```

> 可以用 vscode 的插件 live serve 跑一下两个 html 文件，效果更佳

## referrer 的用处

- document.referrer 这个字段记录了，项目是怎么被打开的（是直接浏览器地址栏打开，还是从某个地方跳转过来打开的）
- 可以统计访问源，或做一些控制，或者可以返回到访问源
