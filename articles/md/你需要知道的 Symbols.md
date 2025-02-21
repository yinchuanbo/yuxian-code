---
title: "你需要知道的 Symbols"
tag: "JavaScript"
time: 2024-09-01 15:21:24
---

## 著名 symbol

著名 symbol 是一个在不同领域中都相同且未注册的 symbol。如果我们要列出著名 symbol，它们会是：

- Symbol.iterator
- Symbol.toStringTag
- Symbol.toPrimitive
- Symbol.asyncIterator
- Symbol.hasInstance
- Symbol.isConcatSpreadable
- Symbol.species
- Symbol.match
- Symbol.matchall
- Symbol.replace
- Symbol.search
- Symbol.split
- Symbol.unscopables
- Symbol.dispose

让我们看一些例子来了解其有用性。

### Symbol.iterator

`Symbol.iterator`：该 symbol 被用来为对象定义默认的迭代器。它被用来在`for-of`循环中实现对对象的迭代，或用于扩展操作符。

```jsx
const obj = { a: 1, b: 2, c: 3 };

obj[Symbol.iterator] = function* () {
  for (const key of Object.keys(this)) {
    yield [key, this[key]];
  }
};

for (const [key, value] of obj) {
  console.log(`${key}: ${value}`);
}
```

### Symbol.toStringTag

`Symbol.toStringTag`：该 symbol 被用来指定在调用`Object.prototype.toString`方法时返回的字符串值，以便为对象提供自定义的字符串表示形式。

```jsx
class MyClass {
  static [Symbol.toStringTag] = "MyClass";
}

const myInstance = new MyClass();

console.log(myInstance.toString()); // outputs '[object MyClass]'
```

### Symbol.toPrimitive

`Symbol.toPrimitive`：该 symbol 被用来指定对象在隐式调用`valueOf`和`toString`方法时的行为。可以用它来为对象提供自定义的字符串和数字表示形式。

```jsx
class Life {
  valueOf() {
    return 42;
  }

  [Symbol.toPrimitive](hint) {
    switch (hint) {
      case "number":
        return this.valueOf();
      case "string":
        return "Forty Two";
      case "default":
        return true;
    }
  }
}

const myLife = new Life();
console.log(+myLife); // 42
console.log(`${myLife}`); // "Forty Two"
console.log(myLife + 0); // 42
```

### Symbol.asyncIterator

`Symbol.asyncIterator`：该 symbol 被用来为对象定义一个异步的迭代器。可以用它来为对象启用异步迭代。

```jsx
class MyAsyncIterable {
  async *[Symbol.asyncIterator]() {
    for (let i = 0; i < 5; i++) {
      await new Promise((resolve) => setTimeout(resolve, 1000));
      yield i;
    }
  }
}

(async () => {
  for await (const value of new MyAsyncIterable()) {
    console.log(value);
  }
})();

// Output after one second:
// 0
// Output after two seconds:
// 1
// Output after three seconds:
// 2
// Output after four seconds:
// 3
// Output after five seconds:
// 4
```

### Symbol.hasInstance

`Symbol.hasInstance`：该 symbol 被用来确认一个对象是否是构造函数的实例。它可以用来更改`instanceof`操作符的行为。

```jsx
class MyArray {
  static [Symbol.hasInstance](instance) {
    return Array.isArray(instance);
  }
}

const arr = [1, 2, 3];
console.log(arr instanceof MyArray); // true
```

### Symbol.isConcatSpreadable

`Symbol.isConcatSpreadable`：该 symbol 被用来确定对象在与其他对象连接时是否应该被展开。它可以用来更改`Array.prototype.concat`方法的行为。

```jsx
const arr1 = [1, 2, 3];
const spreadable = {
  [Symbol.isConcatSpreadable]: true,
  0: 4,
  1: 5,
  2: 6,
  length: 3,
};

console.log([].concat(arr1, spreadable)); // [1, 2, 3, 4, 5, 6]
```

### Symbol.species

`Symbol.species`：该 symbol 被用来指定创建派生对象时要使用的构造函数。它可以用来自定义创建新对象的内置方法的行为。

```jsx
class MyArray extends Array {
  static get [Symbol.species]() {
    return Array;
  }
}

const myArray = new MyArray(1, 2, 3);
const mappedArray = myArray.map((x) => x * 2);

console.log(mappedArray instanceof MyArray); // false
console.log(mappedArray instanceof Array); // true
```

> P.S：这一功能在[未来](https://github.com/tc39/proposal-rm-builtin-subclassing)可能会被删除。

### Symbol.match

`Symbol.match`：该 symbol 被用来在使用`String.prototype.match`方法时确定要搜索的值。它可以用来更改类似于`RegExp`对象的`match`方法的行为。

```jsx
const myRegex = /test/;
"/test/".startsWith(myRegex); // Throws TypeError

const re = /foo/;
re[Symbol.match] = false;
"/foo/".startsWith(re); // true
"/bar/".endsWith(re); // false
```

P.S: 这个 symbol 的存在是标志着一个对象是"regex"的原因。

```jsx
const myRegex = /foo/g;
const str = "How many foos in the the foo foo bar?";

for (result of myRegex[Symbol.matchAll](str)) {
  console.log(result); // we will get the matches
}
```

### Symbol.replace

`Symbol.replace`：该 symbol 被用来在使用`String.prototype.replace`方法时确定替换值。它可以用来更改类似于`RegExp`对象的`replace`方法的行为。

```jsx
const customReplace = (str) => str.replace(/\d+/g, (match) => `-${match}-`);

const customString = {
  [Symbol.replace]: customReplace,
};

const originalString = "foo123bar456baz";

const result = originalString.replace(customString, "*");

console.log(result); // outputs "foo-123-bar-456-baz"
```

### Symbol.split

`Symbol.split`：该 symbol 被用来在使用`String.prototype.split`方法时确定分隔值。它可以用来更改类似于`RegExp`对象的`split`方法的行为。

```jsx
const { Symbol } = require("es6-symbol");

const customSplit = (str) => str.split(/\d+/);

const customRegExp = {
  [Symbol.split]: customSplit,
};

const string = "foo123bar456baz";

string.split(customRegExp); // outputs [ 'foo', 'bar', 'baz' ]
```

### Symbol.unscopables

`Symbol.unscopables`：该 symbol 被用于确定应该从`with`语句的作用域中排除哪些对象属性。它可以用来更改`with`语句的行为。

```jsx
const person = {
  age: 42,
};

person[Symbol.unscopables] = {
  age: true,
};

with (person) {
  console.log(age);
  // Expected output: Error: age is not defined
}
```

### Symbol.dispose

`Symbol.dispose`：“显式资源管理”是指用户通过使用命令式方法（如 Symbol.dispose）或声明式方法（如使用块作用域声明）显式地管理“资源”的生命周期的系统。

```jsx
{
  console.log(1);
  using {
    [Symbol.dispose]() {
      console.log(2);
     }
  };
  console.log(3);
}
// will log 1, 3, 2
```

## 总结

这篇信息性的博客旨在深入介绍 JavaScript 语言中固有的著名 symbol，例如`Symbol.iterator`、`Symbol.toStringTag`和`Symbol.for`。这些 symbol 代表着复杂而多才多艺的工具，可以用来增强和调节代码的行为。在 JavaScript 环境中全面理解可用 symbol 是开发高性能、可维护和可扩展应用程序的关键。因此，在项目的概念化和实施阶段，建议评估将这些 symbol 纳入其中的可行性，以使代码更加简洁、优雅，达到预期的效果。
