---
title: "CSS 中的 var() 函数"
tag: "Css"
time: 2024-09-01 15:21:24
---

## 基础定义

```css
property: var(--variable-name, fallback-value);
```

1. property: 这是要应用值的 CSS 属性。
2. \--variable-name: 这是使用 `--` 前缀定义的 CSS 变量的名称。
3. fallback-value: 这是可选的，表示当变量 `--variable-name` 无效或未定义时的备用值。

CSS 中的 var() 函数允许你使用自定义属性（CSS 变量）来指定属性值。它特别适用于定义可重用的值，比如颜色、字体大小或者任何你希望在整个样式表中重复使用的属性值。以下是一个基本示例：

```css
:root {
  --main-color: #3498db;
}

.element {
  color: var(--main-color);
  background-color: var(--main-color);
}
```

1. `--main-color` 在 `:root` 中全局定义。
2. `var(--main-color)` 用于将颜色应用到 `.element` 类中。

使用带有 var() 的变量有助于保持一致性，并且通过更改一个变量值即可轻松更新整个样式表中的样式。

**在 JavaScript 中，你可以访问和操作使用 CSS 中 var() 函数定义的 CSS 变量：**

**访问 JavaScript 中的 CSS 变量**

可以使用 getComputedStyle 函数与 getPropertyValue 方法来访问 CSS 变量。假设你有相同的 `--main-color` 变量定义

```js
// 获取元素的计算样式
const element = document.querySelector(".element");
const styles = getComputedStyle(element);

// 访问 --main-color 变量的值
const mainColor = styles.getPropertyValue("--main-color");

console.log(mainColor); // 输出：#3498db
```

**设置 JavaScript 中的 CSS 变量**

也可以使用 JavaScript 动态更改 CSS 变量的值：

```js
// 设置 --main-color 变量的值
document.documentElement.style.setProperty("--main-color", "#ff6347");
```

**在 JavaScript 中使用 CSS 变量**

CSS 变量可以用于 JavaScript 中的各种样式操作，它们提供了一种强大的方式来在你的 Web 应用程序中保持一致性并动态更新样式。

CSS 中的 var() 函数用于定义和访问自定义属性，通常称为 CSS 变量。这些变量作用域限定在文档树中，允许在整个样式表中重复使用数值。它们特别有利于通过集中数值于一处，在 Web 设计中保持一致性并简化更新。这一定义突显了 CSS 变量如何优化样式管理，并增强在 Web 项目中的可维护性。
