---
title: "一些 css 语法解释"
tag: "Css"
time: 2024-09-01 15:21:24
---

1. **filter: invert(1)**

是一种 CSS 滤镜效果，它用于将元素的颜色进行反转，即将亮色变为暗色，暗色变为亮色。具体而言，它将元素的亮度值进行反转，使得黑色变为白色，白色变为黑色，其他颜色也会相应地进行反转。这个滤镜效果常用于创建黑暗主题、反色效果或者增强视觉对比度。

2. **\-webkit-autofill**

```css
.contac__input: -webkit-autofill {
  transition: background-color 6000s, color 6000s;
}
```

这段代码是用于自动填充表单输入框的样式设置。 `-webkit-autofill` 是一个 Webkit 浏览器的伪类选择器，用于匹配自动填充的表单字段。

`transition: background-color 6000s, color 6000s;` 是一个过渡效果的设置，它定义了在 6000 秒内发生的过渡效果。具体来说，它定义了在自动填充表单字段时，背景颜色和文字颜色的过渡效果。

这段代码的作用是为自动填充表单字段提供过渡效果，使得填充的背景颜色和文字颜色能够平滑地改变，提升用户体验。

3. **placeholder-shown**

```css
.contact__input:not(:placeholder-shown).contact__input:not(:focus)
  + .contact__label {
  opacity: 1;
  top: -16px;
}
```

这段代码是用于在表单输入框中输入内容时，控制相邻标签的样式变化。具体来说：

`.contact__input:not(:placeholder-shown).contact__input:not(:focus)` 选择器表示选中没有占位符文本且没有获得焦点的输入框。

`+ .contact__label` 表示选中与上述选择器相邻的类名为 `.contact__label` 的元素。

`opacity: 1;` 设置透明度为 1，即完全不透明。

`top: -16px` 将元素的位置向上移动 16 像素。

这段代码的作用是在用户在输入框中输入内容时，将相邻的标签元素显示出来，并将其位置向上移动 16 像素。通常用于实现输入框中的占位标签在输入内容时的动态效果，提升用户体验。

4. **margin-inline**

```css
.contact__data {
  margin-inline: auto;
}
```

这段代码设置了类名为 `.contact__data` 的元素的水平居中对齐。具体来说：

`margin-inline: auto;` 是一个 CSS 属性，用于设置元素的水平外边距。 `auto` 值表示自动计算外边距，使元素在水平方向上居中对齐。

这段代码的作用是将类名为 `.contact__data` 的元素在水平方向上居中对齐。通常用于在布局中使元素水平居中，以提升页面的美观性和可读性。

5. **padding-block**

`padding-block: 5.5rem` 是一个 CSS 属性，用于设置元素的内边距（padding）。具体来说：

`padding-block` 是一个逻辑属性，它根据文本方向自动映射到 `padding-top` 和 `padding-bottom` 属性。

`5.5rem` 是一个长度值，表示以根元素的字体尺寸（root em）为单位的内边距大小。

这段代码的作用是设置元素的上下内边距为 5.5 倍根元素字体尺寸的大小。通过调整元素的内边距，可以控制元素与其内容之间的间距，从而影响元素的布局和外观。

6. **inset: 0**

```css
.nav {
  width: 72px;
  height: max-content;
  position: fixed;
  inset: 0;
  margin: auto 0;
}
```

`inset: 0;` 是一个 CSS 属性，用于设置元素的定位偏移量。具体来说：

`inset` 是一个逻辑属性，它可以同时设置 `top` 、 `right` 、 `bottom` 和 `left` 四个方向的偏移量。

`0` 表示将元素的上、右、下和左四个方向的偏移量都设置为 0。

这段代码的作用是将类名为 `.nav` 的元素固定定位于父容器中，并将其上、右、下和左四个方向的偏移量都设置为 0，即紧贴父容器的上、右、下和左边缘。

通过使用 `inset: 0;` ，可以实现将元素完全填充到父容器中，使其充满整个容器的效果。这在创建全屏导航菜单等布局中非常有用。
