---
title: "使用 JavaScript 加载字体"
tag: "JavaScript"
time: 2024-09-01 15:21:24
---

> 有一个库，叫 Web Font Loader，这个库体积很小，在浏览器支持的情况下，他会使用原生的字体加载 API；在浏览器不支持的情况下，它会模拟相同的功能。这个库内置了一些 web 字体服务，比如 Typekit、Google Fonts 和 Fonts.com，同时也支持自托管的字体。

可以下载这个库，也可以从 Google 的服务器上加载它，地址是：`https://developers.google.com/speed/libraries/#web-font-loader`

**Web Font Loader** 提供了很多有用的功能，其中最有用的就是确保字体加载的跨浏览器一致性。

**Web Font Loader 为以下事件提供了接入点：**

- loading: 开始加载字体
- active: 字体加载完成
- inactive: 字体加载失败

需要把 `@font-face` 块中的所有代码转移到一个独立的样式表 `alegreya-vollkorn.css`，同时把它放在一个子文件夹 css 中，然后，需要在页面头部添加一个小段 Js 代码：

```html
<script>
  WebFontConfig = {
    custom: {
      families: ["AlegreyaSans:n4,i4", "Vollkorn:n6,n5,n7"],
      urls: ["css/alegreya-vollkorn.css"],
    },
  };
  (function () {
    var wf = document.createElement("script");
    wf.src = "https://ajax.googleapis.com/ajax/libs/webfont/1/webfont.js";
    wf.type = "text/javascript";
    wf.async = "true";
    var s = document.getElementsByTagName("script")[0];
    s.parentNode.insertBefore(wf, s);
  })();
</script>
```

这段代码既负责加载 Web Font Loader 脚本，又负责配置后面要使用的字体变体，描述变体的代码中 font-family 名称后面，比如 n4 表示 “normal 样式，400 粗细”，依次类推，在这个样式表中的字体加载后，脚本会自动给 html 元素添加生成的类名。这样，我们就可以在 CSS 中提前编写加载新字体的规则。

```css
body {
  font-family: Helvetica, arial, sans-serif;
}
.wf-alegreya-n4-active body {
  font-family: Alegreya, Helvetica, arial, sans-serif;
}
```

这两条 CSS 规则的含义是，在 Alegreya 字体加载前，使用准备好的后备字体。而在 Alegreya 字体加载后，脚本会给 html 元素添加 `wf-alegreya-n4-active` 类，于是浏览器马上启用新下载的字体。这样不仅能保证浏览器加载字体的一致性，还让我们有机会为后备字体和 Web 字体分别调整版式。

### 匹配后备字体大小

通过在字体加载期间应用类似的规则，可以控制因 Web 字体与后备字体大小不同带来的版本抖动，

在我们的例子中，Alegreya 字体的 x 高度明显小于 Helvetica 和 Arial。通过微调 font-size 和 line-height，可以让它们的高度尽量接近。同理，还可以通过 word-spacing 来微调字符宽度。

```css
.wf-alegreyasans-n4-loading p {
  font-size: 0.905em;
  word-spacing: -0.11em;
  line-height: 1.72;
  font-size-adjust: auto;
}
```

**font-size-adjust** 这个属性用于指定 x 高度与 font-size 的比率，在某个字形缺少合适字体的情况下，后备字体会被调整为该比率。

```css
.wf-alegreyasans-n4-active body {
  font-size-adjust: auto;
}
```
