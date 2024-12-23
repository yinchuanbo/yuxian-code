---
title: "CSS 动画性能优化"
tag: "Css"
time: 2024-09-01 15:21:24
---

## 1\. 使用 transform 和 opacity 属性

别慌，这里有个秘籍。我们可以用`transform`和`opacity`属性来做动画，因为它们能够实现硬件加速，减少重绘和重排。别再用那些会引起布局变化的属性了，比如`width`、`height`，这样只会让浏览器头疼。

```css
.element {
  transform: translateX(100px);
  opacity: 0.5;
  transition: transform 0.3s ease;
}
```

- 具体案例了解一下：

```html
<head>
  <style>
    .box {
      width: 100px;
      height: 100px;
      background-color: red;
      transform: translateX(0);
      opacity: 1;
      transition: transform 0.5s ease, opacity 0.5s ease;
    }
    .box.animate {
      transform: translateX(200px);
      opacity: 0.5;
    }
  </style>
</head>
<body>
  <div class="box" id="box"></div>
  <button onclick="animateBox()">Animate</button>
  <script>
    function animateBox() {
      document.getElementById("box").classList.toggle("animate");
    }
  </script>
</body>
```

点击按钮，你会看到红色方块平滑地移动并逐渐变透明，动画流畅且性能优秀。

## 2\. 优化动画帧率

要掌握好动画的帧率，不能让它太快也不能让它太慢。一般来说，60 帧每秒就很不错了，用`requestAnimationFrame`函数可以帮你控制好动画的更新频率。

```js
function animate() {
  // 动画逻辑
  requestAnimationFrame(animate);
}
animate();
```

- 具体案例了解一下：

```html
<head>
  <style>
    .circle {
      width: 50px;
      height: 50px;
      background-color: blue;
      border-radius: 50%;
      position: absolute;
      top: 50px;
    }
  </style>
</head>
<body>
  <div class="circle" id="circle"></div>
  <script>
    let start = null;
    const element = document.getElementById("circle");
    const duration = 2000; // 动画时长2秒
    function step(timestamp) {
      if (!start) start = timestamp;
      const progress = timestamp - start;
      const translateX = Math.min((progress / duration) * 200, 200);
      element.style.transform = `translateX(${translateX}px)`;
      if (progress < duration) {
        requestAnimationFrame(step);
      }
    }
    requestAnimationFrame(step);
  </script>
</body>
```

这个例子中，小蓝圆会在 2 秒内平滑地移动 200 像素，利用`requestAnimationFrame`保持 60fps。

## 3\. 使用 will-change 属性

听说过`will-change`属性吗？它可以提前告诉浏览器，哪些元素要发生变化，这样浏览器就可以提前做好准备了。但别滥用哦，否则会增加内存消耗。

```css
.element {
  will-change: transform, opacity;
}
```

- 举个栗子：

```html
<head>
  <style>
    .box {
      width: 100px;
      height: 100px;
      background-color: green;
      will-change: transform, opacity;
      transition: transform 0.3s ease, opacity 0.3s ease;
    }
    .box.animate {
      transform: translateY(100px);
      opacity: 0.3;
    }
  </style>
</head>
<body>
  <div class="box" id="box"></div>
  <button onclick="animateBox()">Animate</button>
  <script>
    function animateBox() {
      document.getElementById("box").classList.toggle("animate");
    }
  </script>
</body>
```

这个例子中，点击按钮时，绿色方块会平滑地向下移动并变透明，使用`will-change`优化了动画性能。

## 4\. 避免频繁重绘与重排

动画中样式变化太频繁可不行，这会让浏览器忙个不停。尽量合并多个样式变化，或者使用缓存，让浏览器轻松一点。

举个例子，如果你要改变多个样式属性，应该一次性完成：

```css
.element {
  transform: translateX(0);
  opacity: 1;
  transition: transform 0.5s ease, opacity 0.5s ease;
}
.element.animate {
  transform: translateX(100px);
  opacity: 0.5;
}
```

## 5\. 使用 CSS 变量

使用 CSS 变量可以让你的动画更加灵活和可维护。例如：

```css
:root {
  --translate-distance: 100px;
  --animation-duration: 0.5s;
}
.element {
  transform: translateX(0);
  transition: transform var(--animation-duration) ease;
}
.element.animate {
  transform: translateX(var(--translate-distance));
}
```

通过这种方式，你可以很容易地调整动画的属性，而不需要修改多个地方的代码。