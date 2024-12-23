---
title: "前端 js 动画库"
tag: "工具集"
time: 2024-09-01 15:21:24
---

## gsap

做前端动画，我强推这个框架，真的很好用！

`GSAP`（GreenSock Animation Platform）是一个用于创建`高性能、跨浏览器`的动画的`JavaScript库`。它提供了许多强大而灵活的 API，允许开发者创建各种复杂的动画效果。

使用起来非常简单，我们实现一个动画，将.box 这个元素在 x 轴方向向右移动 200px，耗时 2 秒

```js
gsap.to(".box", {
  x: 200,
  duration: 2,
});
```

当然，这只是 gsap 功能的冰山一角，它还有很多可以让我们高效工作的能力：

1. **CSS 属性动画**：可以对几乎所有 CSS 属性进行动画处理，包括不常用的属性。
2. **时间线控制**：提供时间线功能，可以方便地控制动画的`序列`和并行。
3. **缓动函数**：内置多种`缓动函数`，可以创建更自然的运动效果。
4. **SVG 和 Canvas 动画**：支持`SVG`和`HTML5 Canvas`元素的动画。
5. **复杂动画路径**：可以创建复杂的运动路径，实现复杂的动画效果。
6. **3D 动画**：虽然 GSAP 主要用于 2D 动画，但也支持一些 3D 动画效果。
7. **插件系统**：有丰富的插件系统，可以扩展其功能，比如 MorphSVG 插件可以创建 SVG 图形的形变动画。
8. **性能优化**：GSAP 在性能上做了大量优化，可以处理大量的动画而不影响页面性能。
9. **跨浏览器兼容性**：确保动画在各种浏览器和设备上都能平滑运行。

GSAP 的一个关键特点是它对性能的优化，它使用优化的算法和浏览器的请求动画帧（`requestAnimationFrame`）API 来实现流畅的动画效果。

此外，GSAP 的 API 设计得非常人性化，易于学习和使用，同时它还提供了大量的文档和示例来帮助开发者快速上手。

> gsap 官方地址：[gsap.com](https://gsap.com/)

## lottie

`Lottie` 是一个由`爱彼迎（Airbnb）`开发的开源动画库，主要用法就是 UI 设计通过专业动画软件（如`After Effects`）设计好之后，输出`json`文件，然后通过 Lottie 将这些动画无缝地集成到移动 app 和网页中。

```js
lottie.loadAnimation({
  container: element, // the dom element that will contain the animation
  renderer: "svg",
  loop: true,
  autoplay: true,
  path: "data.json", // the path to the animation json
});
```

Lottie 优势：

1. **After Effects 兼容性**：Lottie 支持将 After Effects 项目（.json 格式的动画数据）直接转换为可在应用和网页中使用的动画。
2. **跨平台**：Lottie 支持多种平台，包括 Android、iOS、Web（通过 React Native、Vue、Angular 等框架）。
3. **性能炸裂**：Lottie 使用原生的图形和动画代码，这意味着动画性能通常比使用 CSS 或 JavaScript 直接编写的动画更优。
4. **定制化**：动画可以被定制和动态修改，例如更改颜色、大小、速度等。
5. **轻量级**：Lottie 动画文件体积小，因为它们只包含关键帧数据，而不是完整的视频或序列帧。
6. **易于使用**：Lottie 提供了简单的 API，使得在项目中集成动画变得非常容易。
7. **动画效果丰富**：由于它基于 After Effects，Lottie 可以支持复杂的动画效果，包括 3D 效果、遮罩、表达式等。
8. **实时渲染**：Lottie 动画是实时渲染的，这意味着动画可以在不同的屏幕尺寸和分辨率下保持高质量。
9. **社区支持**：作为一个开源项目，Lottie 有一个活跃的社区，不断有新的功能和改进被加入。
10. **动画缓存**：Lottie 支持动画的缓存，可以提高重复播放动画的性能。

Lottie 的使用场景非常广泛，从简单的加载指示器到复杂的交互式动画，都可以用 Lottie 来实现。

> Lottie 官方地址：[airbnb.io/lottie/#/we…](https://airbnb.io/lottie/#/web%22)

## React Spring

`React Spring`：react 开发者的福音，这款框架是基于`React`的动画库，它使用基于物理的动画，可以创建非常自然和流畅的动画效果。

初始化一个简单的动画，让 div 从左向右移动：

```js
import { useSpring, animated } from "@react-spring/web";
export default function MyComponent() {
  const springs = useSpring({
    from: { x: 0 },
    to: { x: 100 },
  });
  return (
    <animated.div
      style={{
        width: 80,
        height: 80,
        background: "#ff6d6d",
        borderRadius: 8,
        ...springs,
      }}
    />
  );
}
```

> React Spring 官方地址：[www.react-spring.dev/docs/gettin…](https://www.react-spring.dev/docs/getting-started)

## Anime.js

`Anime.js`：一个`轻量级`的 JavaScript 动画库，它使用`CSS`属性和`SVG`，可以创建平滑的 CSS 和 SVG 动画

初始化一个简单的动画：

```js
anime({
  targets: ".css-prop-demo .el",
  left: "240px",
  backgroundColor: "#FFF",
  borderRadius: ["0%", "50%"],
  easing: "easeInOutQuad",
});
```

说实话，看 api，有点类似于 gsap 动画库，也不知道它倆是怎么个关系~

> Anime.js 官方地址：[animejs.com/documentati…](https://animejs.com/documentation/#cssSelector)

## 其他动画库

还有很多其他的动画库，但是作者本人还没用过，如果有用过的小伙伴也可以在评论区发表一下使用体验或者看法。

1. **Framer Motion**：也是一个基于`React`的动画库，它提供了一个简单的 API 来创建动画和交互效果。
2. **Velocity.js**：一个强大的动画引擎，可以与`jQuery`结合使用，提供丰富的动画效果。
3. **Popmotion**：一个功能全面的动作和动画库，支持 React 和非 React 环境。
4. **Web Animations API**：是现代浏览器提供的原生 JavaScript 动画 API，允许开发者以编程方式创建复杂的动画。
5. **KUTE.js**：一个轻量级的动画库，专注于性能和易用性，支持 CSS 属性、SVG 和自定义属性的动画。
6. **Flickity**：虽然主要是一个轻量级的画廊和卡丁车库，但它也提供了一些基本的动画功能。
7. **GreenSock Draggable**：GSAP 的一个插件，允许元素的拖放，可以创建交互式的动画效果。
8. **CyberConnect**：一个基于 Web Animations API 的库，可以创建复杂的动画和过渡效果。
