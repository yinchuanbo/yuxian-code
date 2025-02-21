---
title: "CSS 属性 appearance"
tag: "Css"
time: 2024-09-01 15:21:24
---

## `appearance`：跨越原生与定制界限的桥梁

`appearance`属性在 Web 设计师的工具箱中可能尚属生僻，它能够移除或者添加操作系统和浏览器默认提供的样式。在创建定制化表单控件，如按钮、选择框、文本输入框等时，经常需要涉及到的是如何去除浏览器预设的样式，以便控件的外观更贴合设计稿。横亘在设计与实现之间的障碍，便是如何处理浏览器默认样式的复杂性。在这种情况下，`appearance`属性就显得尤为有用。

### 解决痛点

在使用复杂的 CSS 重写浏览器样式之前，`appearance`可以作为第一步，移除浏览器默认样式，无须担忧控件在不同浏览器下的一致性问题。确切地说，这个属性能够让元素不再使用平台原生的样式，而是采用应用层定义的样式，这对于统一用户界面提供了极大的便利。

### 属性解析

`appearance`属性的可能取值相对简单：

- `none`：消除元素的默认样式。
- `auto`（默认值）：元素保持默认样式。
- `menulist-button`、`textfield` 等浏览器允许的具体值，允许特定元素有特定的样式。

### 具体应用

假设在一个线上图书商店的界面上，有一组单选按钮用于筛选图书类别，而设计稿中的按钮样式与浏览器默认样式大相径庭。一种简明的实现方式是先用`appearance`属性去除默认的单选按钮样式，然后应用定制的 CSS 样式。

```css
/* Reset the default style of radio buttons */
.radio-button {
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;

  /* Custom styles */
  width: 20px;
  height: 20px;
  border: 2px solid #5c6ac4;
  border-radius: 50%;
  background-color: transparent;
}

/* Style for checked state */
.radio-button:checked {
  background-color: #5c6ac4;
}

/* The label style for customization*/
.label {
  display: inline-block;
  margin-left: 8px;
}
```

```html
<label>
  <input
    type="radio"
    class="radio-button"
    name="book-category"
    value="fiction"
  />
  <span class="label">Fiction</span>
</label>

<label>
  <input
    type="radio"
    class="radio-button"
    name="book-category"
    value="non-fiction"
  />
  <span class="label">Non-fiction</span>
</label>
```

在上面的示例中，应用了`appearance: none;`之后，单选按钮的默认样式被去除，这就为添加新的样式提供了一个白板。接着，通过 CSS 我们定义了按钮的大小、边框、背景色以及在选中状态下的样式。

## 兼容性与使用建议

`appearance`属性的跨浏览器支持目前已经相当好，多数现代浏览器都已经支持非前缀或带有前缀的`appearance`样式。然而，鉴于 Web 世界的多样化和动态性，设计者和开发者在采用此属性前仍需检查各自目标用户所使用的浏览器版本的支持情况。

同样需要注意的是，`appearance`属性并非万能钥匙。某些情况下，仅凭`appearance`可能不足以实现高度定制化设计，可能还需要通过其他 CSS 技巧来进一步细化控件样式。
