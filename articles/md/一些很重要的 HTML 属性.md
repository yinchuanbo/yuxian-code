---
title: "一些很重要的 HTML 属性"
tag: "Html"
time: 2024-09-01 15:21:24
---

## inputmode

inputmode 属性可以定义 input 或者 textarea 元素弹出键盘的类型。

这对于移动端开发还是很实用的

```html
<!-- 数字键盘 numeric -->
<input type="text" inputmode="numeric" />
<!-- 手机号键盘 tel -->
<input type="text" inputmode="tel" />
<!-- 邮箱 email -->
<input type="text" inputmode="email" />
<!-- 链接 url -->
<input type="text" inputmode="url" />
<!-- 搜索 search (键盘会出现 Go/确认/Return) -->
<input type="text" inputmode="search" />
```

## poster

```html
<video src="" poster="preview.png"></video>
```

## multiple

multiple 通常用于 input 标签文本选择时的多选功能，如：

```html
<input type="file" id="files" name="files" multiple />
```

除此之外，还可以用于 select 标签多选：

```html
<label for="cars">请选择一个汽车品牌：</label>
<select name="cars" id="cars" multiple>
  <option value="audi">奥迪</option>
  <option value="byd">比亚迪</option>
  <option value="geely">吉利</option>
  <option value="volvo">沃尔沃</option>
</select>
```

## accesskey

accesskey 用于规定快捷键，用于 **激活/聚焦** 元素，比如下面一个超链接：

```html
<a href="https://juejin.cn/" accesskey="j"></a>
```

如果你的浏览器是谷歌的话，直接按下 `alt + j` 就会跳转到掘金链接。当然，激活 accesskey 的操作取决于浏览器及其平台。

## tabindex

tabindex 可以指示其元素是否可以聚焦，以及它是否/在何处参与顺序键盘导航。

```html
<input tabindex="2" type="text" />
<div tabindex="0">可以聚焦的 div</div>
<input tabindex="1" type="text" />
```

## download

download 一般用于 a 标签中，可以让浏览器直接进行下载，而不是展示链接内容，

```html
<a href="./a.jpg" download>下载</a>
```

点击就会下载 `a.jpg`，如果你想改变下载文件名，可以这样写：

```html
<a href="./a.jpg" download="b.jpg">下载</a>
```

这样下载的文件名就改变了。

注意：download 如果下载跨域资源的话就会失败。
