---
title: "Eslint 配置指南"
tag: "前端工程化"
time: 2024-09-01 15:21:24
---

1. Eslint 配置指南

ESLint 是一个强大的代码检查工具，用于识别 JavaScript 和其他支持的语言中的常见编程错误，并强制执行一致的编码风格。

ESLint 最初是由 Nicholas C. Zakas 于 2013 年 6 月创建的开源项目。

ESLint 是一个开源的 JavaScript 代码检查工具，它是用来进行代码的校验，检测代码中潜在的问题，比如某个变量定义了未使用、函数定义的参数重复、变量名没有按规范命名等等。

中文官网：[https://zh-hans.eslint.org/](https://zh-hans.eslint.org/)

英文官网：[https://eslint.org/](https://eslint.org/)

下面是一个基本的 ESLint 配置指南，帮助你开始使用它来提升代码质量。

1.1. 安装 ESLint

首先，你需要在项目中安装 ESLint。如果你的项目是一个 Node.js 应用，通常会将其作为开发依赖安装：

```sh
npm install eslint --save-dev
# 或者使用 yarn
yarn add eslint --dev
```

1.2. 生成配置文件

ESLint 支持多种格式的配置文件，包括 `.eslintrc.js`, `.eslintrc.yaml`, .`eslintrc.yml`, `.eslintrc.json`, 以及直接在 `package.json` 中的 `eslintConfig` 字段。你可以根据需要选择合适的格式。

要生成配置文件，可以运行：

```sh
npx eslint --init
```

这会引导你进行一系列问题的回答，根据你的回答自动生成配置。

例如，你可以选择你的环境（浏览器、Node.js 等）、想要遵循的编码规范（如 Airbnb、Google、Standard 等）以及是否要支持某些语言特性（比如 Vue.js 或 React）。

1.3. 修改配置文件

生成的 `.eslintrc.*` 文件包含了 ESLint 的基础配置。你可以手动编辑这个文件来调整规则，添加或移除规则，或改变规则的严格程度。配置文件通常看起来像这样：

```js
// .eslintrc.js 示例
// @see https://eslint.bootcss.com/docs/rules/
module.exports = {
  env: {
    browser: true,
    es2021: true,
    node: true,
    jest: true,
  },
  /* 指定如何解析语法 */
  parser: "vue-eslint-parser",
  /** 优先级低于 parse 的语法解析配置 */
  parserOptions: {
    ecmaVersion: "latest",
    sourceType: "module",
    parser: "@typescript-eslint/parser",
    jsxPragma: "React",
    ecmaFeatures: {
      jsx: true,
    },
  },
  /* 继承已有的规则 */
  extends: [
    "eslint:recommended",
    "plugin:vue/vue3-essential",
    "plugin:@typescript-eslint/recommended",
    "plugin:prettier/recommended",
  ],
  plugins: ["vue", "@typescript-eslint"],
  /*
   * "off" 或 0    ==>  关闭规则
   * "warn" 或 1   ==>  打开的规则作为警告（不影响代码执行）
   * "error" 或 2  ==>  规则作为一个错误（代码不能执行，界面报错）
   */
  rules: {
    // eslint（https://eslint.bootcss.com/docs/rules/）
    "no-var": "error", // 要求使用 let 或 const 而不是 var
    "no-multiple-empty-lines": ["warn", { max: 1 }], // 不允许多个空行
    "no-console": process.env.NODE_ENV === "production" ? "error" : "off",
    "no-debugger": process.env.NODE_ENV === "production" ? "error" : "off",
    "no-unexpected-multiline": "error", // 禁止空余的多行
    "no-useless-escape": "off", // 禁止不必要的转义字符

    // typeScript (https://typescript-eslint.io/rules)
    "@typescript-eslint/no-unused-vars": "error", // 禁止定义未使用的变量
    "@typescript-eslint/prefer-ts-expect-error": "error", // 禁止使用 @ts-ignore
    "@typescript-eslint/no-explicit-any": "off", // 禁止使用 any 类型
    "@typescript-eslint/no-non-null-assertion": "off",
    "@typescript-eslint/no-namespace": "off", // 禁止使用自定义 TypeScript 模块和命名空间。
    "@typescript-eslint/semi": "off",

    // eslint-plugin-vue (https://eslint.vuejs.org/rules/)
    "vue/multi-word-component-names": "off", // 要求组件名称始终为 “-” 链接的单词
    "vue/script-setup-uses-vars": "error", // 防止<script setup>使用的变量<template>被标记为未使用
    "vue/no-mutating-props": "off", // 不允许组件 prop的改变
    "vue/attribute-hyphenation": "off", // 对模板中的自定义组件强制执行属性命名样式
  },
};
```

1.4. 创建 `.eslintignore` 文件

你可以创建一个 `.eslintignore` 文件来指定 ESLint 应该忽略哪些文件或目录。这与 `.gitignore` 类似，每一行一个路径模式。

例如：

```sh
/node_modules/
/dist/
/build/
```

1.5. 运行 ESLint

安装并配置好后，可以在命令行中运行 ESLint 来检查你的代码：

```sh
npx eslint yourfile.js
# 或者检查整个项目
npx eslint .
```

1.6. 整合到编辑器/IDE

大多数现代代码编辑器（如 VSCode, WebStorm 等）都支持 ESLint 插件，允许你在编写代码时实时看到 ESLint 的警告和错误，并提供快速修复选项。确保安装对应的插件并启用它。

1.7. 自动修复

ESLint 提供了一个自动修复功能，可以尝试修复一些简单的警告和错误：

```sh
npx eslint --fix yourfile.js
# 或者修复整个项目
npx eslint --fix .
```

package.json 新增两个运行脚本

```json
{
  "scripts": {
    "lint": "eslint src",
    "fix": "eslint src --fix"
  }
}
```

以上就是 ESLint 的基本配置指南。随着项目的进展，你可能需要根据团队的编码规范进一步定制配置。记得定期更新 ESLint 及其插件以获取最新的规则和支持。

2. 配置 prettier

有了 eslint，为什么还要有 prettier？

eslint 针对的是 javascript，是一个检测工具，包含 js 语法以及少部分格式问题，在 eslint 看来，语法对了就能保证代码正常运行，格式问题属于其次；

而 prettier 属于格式化工具，它看不惯格式不统一，所以它就把 eslint 没干好的事接着干，另外，prettier 支持包含 js 在内的多种语言。

总结起来，eslint 和 prettier 这俩兄弟一个保证 js 代码质量，一个保证代码美观。

来到 VS Code 安装其对应的插件。

2.1. 安装依赖包

```sh
npm install -D eslint-plugin-prettier prettier eslint-config-prettier
yarn add -D eslint-plugin-prettier prettier eslint-config-prettier
pnpm install -D eslint-plugin-prettier prettier eslint-config-prettier
```

同样是在 src 同级目录下安装.prettierrc.json 文件夹

2.2. `.prettierrc.json` 添加规则

```json
{
  "printWidth": 120,
  "tabWidth": 2,
  "useTabs": false,
  "semi": false,
  "singleQuote": true,
  "bracketSpacing": true,
  "trailingComma": "all",
  "jsxSingleQuote": false,
  "arrowParens": "avoid",
  "proseWrap": "preserve",
  "htmlWhitespaceSensitivity": "strict",
  "endOfLine": "auto"
}
```

2.3. `.prettierignore` 忽略文件

```sh
/dist/*
/html/*
.local
/node_modules/**
**/*.svg
**/*.sh
/public/*
```

通过 `pnpm run lint` 去检测语法，如果出现不规范格式，通过 `pnpm run fix` 修改

2.4. 保存自动格式化

想要按 `Ctrl+S` 保证的自动按 eslint 规则格式化代码的话，找到 `.vscode` 文件，内部新建个 `setting.json` 文件夹

setting.json 写入代码

```json
{
  "editor.formatOnType": true,
  "editor.formatOnSave": true,
  "eslint.codeAction.showDocumentation": {
    "enable": true
  },
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "eslint.validate": ["javascript", "javascriptreact", "html", "vue"]
}
```

3. eslint-config-win

此规则适用于 `JavaScript/Vue` 项目的 ESLint 配置规范。

目前已支持 `Vue 3.0`，需要指定 extends 配置 vue3

[https://gitee.com/braden/eslint-config-win?spm=5176.28575747.0.0.ff1f2adcn7ydP7](https://gitee.com/braden/eslint-config-win?spm=5176.28575747.0.0.ff1f2adcn7ydP7)

3.1. 安装

```sh
yarn add @winner-fed/eslint-config-win -D
```

3.2. 依赖版本

```sh
eslint ^7.32.0
@babel/eslint-parser ^7.15.8
vue-eslint-parser ^7.11.0
eslint-plugin-vue ^7.20.0
```

Tips：如果项目中没有安装此依赖包或者版本不一致，请安装或者升级。

3.3. 用法

- 在你的项目的根目录下创建一个 `.eslintrc.js` 文件，并将以下内容复制进去：

```js
module.exports = {
  extends: ["@winner-fed/win"],
  env: {
    // 你的环境变量（包含多个预定义的全局变量）
    //
    // browser: true,
    // node: true,
    // mocha: true,
    // jest: true,
    // jquery: true
  },
  globals: {
    // 你的全局变量（设置为 false 表示它不允许被重新赋值）
    //
    // myGlobal: false
  },
  rules: {
    // 自定义你的规则
  },
};
```

- 项目目录下的 package.json 添加检测指令，举个例子

```json
{
//  ...
 "scripts": {
   + "lint:es": "eslint \"src/**/*.{vue,js,jsx}\" --fix",
 }
//  ...
}
```

3.3.1. Vue

```sh
npm install --save-dev eslint babel-eslint vue-eslint-parser eslint-plugin-vue @winner-fed/eslint-config-win
```

在你的项目的根目录下创建一个 `.eslintrc.js` 文件，并将以下内容复制进去：

```js
module.exports = {
  extends: [
    "@winner-fed/win",
    // 这里是针对 vue2 的配置
    "@winner-fed/win/vue",
    // 如果是 vue3 的项目工程，则推荐下面配置
    // '@winner-fed/win/vue3',
  ],
  env: {
    // 你的环境变量（包含多个预定义的全局变量）
    //
    // browser: true,
    // node: true,
    // mocha: true,
    // jest: true,
    // jquery: true
  },
  globals: {
    // 你的全局变量（设置为 false 表示它不允许被重新赋值）
    //
    // myGlobal: false
  },
  rules: {
    // 自定义你的规则
  },
};
```

3.4. Vue3 新增规则

| rule                                                                                                                   | 规则描述                                             |
| ---------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------- |
| [vue/custom-event-name-casing](https://eslint.vuejs.org/rules/custom-event-name-casing.html)                           | 自定义事件名必须用 kebab-case 风格                   |
| [vue/no-arrow-functions-in-watch](https://eslint.vuejs.org/rules/no-arrow-functions-in-watch.html)                     | watch 中禁止使用箭头函数                             |
| [vue/no-custom-modifiers-on-v-model](https://eslint.vuejs.org/rules/no-custom-modifiers-on-v-model.html)               | 禁止自定义的 v-modal 修饰语                          |
| [vue/no-deprecated-data-object-declaration](https://eslint.vuejs.org/rules/no-deprecated-data-object-declaration.html) | 禁止在 data 中使用已废弃的对象定义                   |
| [vue/no-deprecated-destroyed-lifecycle](https://eslint.vuejs.org/rules/no-deprecated-destroyed-lifecycle.html)         | 禁止使用已废弃的 destroyed 和 beforeDestroy 生命周期 |
| [vue/no-deprecated-dollar-listeners-api](https://eslint.vuejs.org/rules/no-deprecated-dollar-listeners-api.html)       | 禁止使用已废弃的 $listeners                          |
| [vue/no-deprecated-dollar-scopedslots-api](https://eslint.vuejs.org/rules/no-deprecated-dollar-scopedslots-api.html)   | 禁止使用已废弃的 $scopedSlots                        |
| [vue/no-deprecated-events-api](https://eslint.vuejs.org/rules/no-deprecated-events-api.html)                           | 禁止使用已废弃的 events 接口                         |
| [vue/no-deprecated-filter](https://eslint.vuejs.org/rules/no-deprecated-filter.html)                                   | 禁止使用已废弃的 filters 语法                        |
| [vue/no-deprecated-functional-template](https://eslint.vuejs.org/rules/no-deprecated-functional-template.html)         | 禁止使用已废弃的 functional 模版                     |
| [vue/no-deprecated-html-element-is](https://eslint.vuejs.org/rules/no-deprecated-html-element-is.html)                 | 禁止使用已废弃的 is 属性                             |
| [vue/no-deprecated-inline-template](https://eslint.vuejs.org/rules/no-deprecated-inline-template.html)                 | 禁止使用已废弃的 inline-template 属性                |
| [vue/no-deprecated-props-default-this](https://eslint.vuejs.org/rules/no-deprecated-props-default-this.html)           | 禁止使用已废弃的 this                                |
| [vue/no-deprecated-v-bind-sync](https://eslint.vuejs.org/rules/no-deprecated-v-bind-sync.html)                         | 禁止在 v-bind 指令中使用已废弃的 .sync 修饰符        |
| [vue/no-deprecated-v-on-native-modifier](https://eslint.vuejs.org/rules/no-deprecated-v-on-native-modifier.html)       | 禁止使用已废弃的 .native 修饰符                      |
| [vue/no-deprecated-v-on-number-modifiers](https://eslint.vuejs.org/rules/no-deprecated-v-on-number-modifiers.html)     | 禁止使用已废弃的数字修饰符                           |
| [vue/no-deprecated-vue-config-keycodes](https://eslint.vuejs.org/rules/no-deprecated-vue-config-keycodes.html)         | 禁止使用已废弃的 Vue.config.keyCodes                 |
| [vue/no-dupe-v-else-if](https://eslint.vuejs.org/rules/no-dupe-v-else-if.html)                                         | 禁止在 v-if 和 v-else-if 中出现重复的测试表达式      |
| [vue/no-duplicate-attr-inheritance](https://eslint.vuejs.org/rules/no-duplicate-attr-inheritance.html)                 | 使用 v-bind=“$attrs” 时 inheritAttrs 必须是 false    |
| [vue/no-empty-component-block](https://eslint.vuejs.org/rules/no-empty-component-block.html)                           | 禁止 `<template> <script> <style>` 为空              |
| [vue/no-invalid-model-keys](https://eslint.vuejs.org/rules/no-invalid-model-keys.html)                                 | 禁止 model 中出现错误的属性                          |
| [vue/no-lifecycle-after-await](https://eslint.vuejs.org/rules/no-lifecycle-after-await.html)                           | 禁止异步注册生命周期                                 |
| [vue/no-lone-template](https://eslint.vuejs.org/rules/no-lone-template.html)                                           | 禁止出现没必要的 `<template>`                        |
| [vue/no-multiple-objects-in-class](https://eslint.vuejs.org/rules/no-multiple-objects-in-class.html)                   | 禁止 class 中出现复数的对象                          |
| [vue/no-multiple-slot-args](https://eslint.vuejs.org/rules/no-multiple-slot-args.html)                                 | 禁止给 scoped slots 传递多个参数                     |
| [vue/no-multiple-template-root](https://eslint.vuejs.org/rules/no-multiple-template-root.html)                         | 禁止模版中有多个根节点                               |
| [vue/no-mutating-props](https://eslint.vuejs.org/rules/no-mutating-props.html)                                         | 禁止修改组件的 props                                 |
| [vue/no-ref-as-operand](https://eslint.vuejs.org/rules/no-ref-as-operand.html)                                         | 禁止直接使用由 ref 生成的变量，必须使用它的 value    |
| [vue/no-setup-props-destructure](https://eslint.vuejs.org/rules/no-setup-props-destructure.html)                       | 禁止对 setup 中的 props 解构                         |
| [vue/no-sparse-arrays](https://eslint.vuejs.org/rules/no-sparse-arrays.html)                                           | 禁止在数组中出现连续的逗号                           |
| [vue/no-useless-concat](https://eslint.vuejs.org/rules/no-useless-concat.html)                                         | 禁止没必要的字符拼接                                 |
| [vue/no-useless-mustaches](https://eslint.vuejs.org/rules/no-useless-mustaches.html)                                   | 禁止出现无用的 mustache 字符串                       |
| [vue/no-useless-v-bind](https://eslint.vuejs.org/rules/no-useless-v-bind.html)                                         | 禁止出现无用的 v-bind                                |
| [vue/no-v-for-template-key](https://eslint.vuejs.org/rules/no-v-for-template-key.html)                                 | 禁止 template 有 v-for 属性时又有 key 属性           |
| [vue/no-v-for-template-key-on-child](https://eslint.vuejs.org/rules/no-v-for-template-key-on-child.html)               | 禁止 template v-for 属性的子节点有 key 属性          |
| [vue/no-watch-after-await](https://eslint.vuejs.org/rules/no-watch-after-await.html)                                   | 禁止在 await 之后调用 watch                          |
| [vue/one-component-per-file](https://eslint.vuejs.org/rules/one-component-per-file.html)                               | 一个文件必须仅包含一个组件                           |
| [vue/order-in-components](https://eslint.vuejs.org/rules/order-in-components.html)                                     | 组件的属性必须为一定的顺序                           |
| [vue/require-explicit-emits](https://eslint.vuejs.org/rules/require-explicit-emits.html)                               | emits 属性必须包含 $emit() 中的值                    |
| [vue/require-slots-as-functions](https://eslint.vuejs.org/rules/require-slots-as-functions.html)                       | this.$slots.default 必须被当作方法使用               |
| [vue/require-toggle-inside-transition](https://eslint.vuejs.org/rules/require-toggle-inside-transition.html)           | transition 内部必须有条件指令                        |
| [vue/return-in-emits-validator](https://eslint.vuejs.org/rules/this-in-template.html)                                  | emits 中的方法必须有返回值                           |
| [vue/v-on-event-hyphenation](https://eslint.vuejs.org/rules/v-on-event-hyphenation.html)                               | 禁止在 v-on 的事件名使用横杠                         |
| [vue/valid-v-is](https://eslint.vuejs.org/rules/valid-v-is.html)                                                       | v-is 指令必须合法                                    |

3.6. 开发维护

以测试开发驱动，`config/rules/*.json` 文件都是根据 `test/` 文件夹对应的生成的

4. eslint 配置举例

```json
{
  "rules": {
    // 定义对象的set存取器属性时，强制定义get
    "accessor-pairs": 2,
    // 指定数组的元素之间要以空格隔开(,后面)， never参数：[ 之前和 ] 之后不能带空格，always参数：[ 之前和 ] 之后必须带空格
    "array-bracket-spacing": [2, "never"],
    // 在块级作用域外访问块内定义的变量是否报错提示
    "block-scoped-var": 0,
    // if while function 后面的{必须与if在同一行，java风格。
    "brace-style": [2, "1tbs", { "allowSingleLine": true }],
    // 双峰驼命名格式
    "camelcase": 2,
    // 数组和对象键值对最后一个逗号， never参数：不能带末尾的逗号, always参数：必须带末尾的逗号，
    // always-multiline：多行模式必须带逗号，单行模式不能带逗号
    "comma-dangle": [2, "never"],
    // 控制逗号前后的空格
    "comma-spacing": [2, { "before": false, "after": true }],
    // 控制逗号在行尾出现还是在行首出现
    // http://eslint.org/docs/rules/comma-style
    "comma-style": [2, "last"],
    // 圈复杂度
    "complexity": [2, 9],
    // 以方括号取对象属性时，[ 后面和 ] 前面是否需要空格, 可选参数 never, always
    "computed-property-spacing": [2, "never"],
    // 强制方法必须返回值，TypeScript强类型，不配置
    "consistent-return": 0,
    // 用于指统一在回调函数中指向this的变量名，箭头函数中的this已经可以指向外层调用者，应该没卵用了
    // e.g [0,"that"] 指定只能 var that = this. that不能指向其他任何值，this也不能赋值给that以外的其他值
    "consistent-this": 0,
    // 强制在子类构造函数中用super()调用父类构造函数，TypeScrip的编译器也会提示
    "constructor-super": 0,
    // if else while for do后面的代码块是否需要{ }包围，参数：
    //    multi  只有块中有多行语句时才需要{ }包围
    //    multi-line  只有块中有多行语句时才需要{ }包围, 但是块中的执行语句只有一行时，
    //                   块中的语句只能跟和if语句在同一行。if (foo) foo++; else doSomething();
    //    multi-or-nest 只有块中有多行语句时才需要{ }包围, 如果块中的执行语句只有一行，执行语句可以零另起一行也可以跟在if语句后面
    //    [2, "multi", "consistent"] 保持前后语句的{ }一致
    //    default: [2, "all"] 全都需要{ }包围
    "curly": [2, "all"],
    // switch语句强制default分支，也可添加 // no default 注释取消此次警告
    "default-case": 2,
    // 强制object.key 中 . 的位置，参数:
    //      property，'.'号应与属性在同一行
    //      object, '.' 号应与对象名在同一行
    "dot-location": [2, "property"],
    // 强制使用.号取属性
    //    参数： allowKeywords：true 使用保留字做属性名时，只能使用.方式取属性
    //                          false 使用保留字做属性名时, 只能使用[]方式取属性 e.g [2, {"allowKeywords": false}]
    //           allowPattern:  当属性名匹配提供的正则表达式时，允许使用[]方式取值,否则只能用.号取值 e.g [2, {"allowPattern": "^[a-z]+(_[a-z]+)+$"}]
    "dot-notation": [2, { "allowKeywords": true }],
    // 文件末尾强制换行
    "eol-last": 2,
    // 使用 === 替代 ==
    "eqeqeq": [2, "allow-null"],
    // 方法表达式是否需要命名
    "func-names": 0,
    // 方法定义风格，参数：
    //    declaration: 强制使用方法声明的方式，function f(){} e.g [2, "declaration"]
    //    expression：强制使用方法表达式的方式，var f = function() {}  e.g [2, "expression"]
    //    allowArrowFunctions: declaration风格中允许箭头函数。 e.g [2, "declaration", { "allowArrowFunctions": true }]
    "func-style": 0,
    "no-alert": 0, //禁止使用alert confirm prompt
    "no-array-constructor": 2, //禁止使用数组构造器
    "no-bitwise": 0, //禁止使用按位运算符
    "no-caller": 1, //禁止使用arguments.caller或arguments.callee
    "no-catch-shadow": 2, //禁止catch子句参数与外部作用域变量同名
    "no-class-assign": 2, //禁止给类赋值
    "no-cond-assign": 2, //禁止在条件表达式中使用赋值语句
    "no-console": 2, //禁止使用console
    "no-const-assign": 2, //禁止修改const声明的变量
    "no-constant-condition": 2, //禁止在条件中使用常量表达式 if(true) if(1)
    "no-continue": 0, //禁止使用continue
    "no-control-regex": 2, //禁止在正则表达式中使用控制字符
    "no-debugger": 2, //禁止使用debugger
    "no-delete-var": 2, //不能对var声明的变量使用delete操作符
    "no-div-regex": 1, //不能使用看起来像除法的正则表达式/=foo/
    "no-dupe-keys": 2, //在创建对象字面量时不允许键重复 {a:1,a:1}
    "no-dupe-args": 2, //函数参数不能重复
    "no-duplicate-case": 2, //switch中的case标签不能重复
    "no-else-return": 2, //如果if语句里面有return,后面不能跟else语句
    "no-empty": 2, //块语句中的内容不能为空
    "no-empty-character-class": 2, //正则表达式中的[]内容不能为空
    "no-empty-label": 2, //禁止使用空label
    "no-eq-null": 2, //禁止对null使用==或!=运算符
    "no-eval": 1, //禁止使用eval
    "no-ex-assign": 2, //禁止给catch语句中的异常参数赋值
    "no-extend-native": 2, //禁止扩展native对象
    "no-extra-bind": 2, //禁止不必要的函数绑定
    "no-extra-boolean-cast": 2, //禁止不必要的bool转换
    "no-extra-parens": 2, //禁止非必要的括号
    "no-extra-semi": 2, //禁止多余的冒号
    "no-fallthrough": 1, //禁止switch穿透
    "no-floating-decimal": 2, //禁止省略浮点数中的0 .5 3.
    "no-func-assign": 2, //禁止重复的函数声明
    "no-implicit-coercion": 1, //禁止隐式转换
    "no-implied-eval": 2, //禁止使用隐式eval
    "no-inline-comments": 0, //禁止行内备注
    "no-inner-declarations": [2, "functions"], //禁止在块语句中使用声明（变量或函数）
    "no-invalid-regexp": 2, //禁止无效的正则表达式
    "no-invalid-this": 2, //禁止无效的this，只能用在构造器，类，对象字面量
    "no-irregular-whitespace": 2, //不能有不规则的空格
    "no-iterator": 2, //禁止使用__iterator__ 属性
    "no-label-var": 2, //label名不能与var声明的变量名相同
    "no-labels": 2, //禁止标签声明
    "no-lone-blocks": 2, //禁止不必要的嵌套块
    "no-lonely-if": 2, //禁止else语句内只有if语句
    "no-loop-func": 1, //禁止在循环中使用函数（如果没有引用外部变量不形成闭包就可以）
    "no-mixed-requires": [0, false], //声明时不能混用声明类型
    "no-mixed-spaces-and-tabs": [2, false], //禁止混用tab和空格
    "linebreak-style": [0, "windows"], //换行风格
    "no-multi-spaces": 1, //不能用多余的空格
    "no-multi-str": 2, //字符串不能用\换行
    "no-multiple-empty-lines": [1, { "max": 2 }], //空行最多不能超过2行
    "no-native-reassign": 2, //不能重写native对象
    "no-negated-in-lhs": 2, //in 操作符的左边不能有!
    "no-nested-ternary": 0, //禁止使用嵌套的三目运算
    "no-new": 1, //禁止在使用new构造一个实例后不赋值
    "no-new-func": 1, //禁止使用new Function
    "no-new-object": 2, //禁止使用new Object()
    "no-new-require": 2, //禁止使用new require
    "no-new-wrappers": 2, //禁止使用new创建包装实例，new String new Boolean new Number
    "no-obj-calls": 2, //不能调用内置的全局对象，比如Math() JSON()
    "no-octal": 2, //禁止使用八进制数字
    "no-octal-escape": 2, //禁止使用八进制转义序列
    "no-param-reassign": 2, //禁止给参数重新赋值
    "no-path-concat": 0, //node中不能使用__dirname或__filename做路径拼接
    "no-plusplus": 0, //禁止使用++，--
    "no-process-env": 0, //禁止使用process.env
    "no-process-exit": 0, //禁止使用process.exit()
    "no-proto": 2, //禁止使用__proto__属性
    "no-redeclare": 2, //禁止重复声明变量
    "no-regex-spaces": 2, //禁止在正则表达式字面量中使用多个空格 /foo bar/
    "no-restricted-modules": 0, //如果禁用了指定模块，使用就会报错
    "no-return-assign": 1, //return 语句中不能有赋值表达式
    "no-script-url": 0, //禁止使用javascript:void(0)
    "no-self-compare": 2, //不能比较自身
    "no-sequences": 0, //禁止使用逗号运算符
    "no-shadow": 2, //外部作用域中的变量不能与它所包含的作用域中的变量或参数同名
    "no-shadow-restricted-names": 2, //严格模式中规定的限制标识符不能作为声明时的变量名使用
    "no-spaced-func": 2, //函数调用时 函数名与()之间不能有空格
    "no-sparse-arrays": 2, //禁止稀疏数组， [1,,2]
    "no-sync": 0, //nodejs 禁止同步方法
    "no-ternary": 0, //禁止使用三目运算符
    "no-trailing-spaces": 1, //一行结束后面不要有空格
    "no-this-before-super": 0, //在调用super()之前不能使用this或super
    "no-throw-literal": 2, //禁止抛出字面量错误 throw "error";
    "no-undef": 1, //不能有未定义的变量
    "no-undef-init": 2, //变量初始化时不能直接给它赋值为undefined
    "no-undefined": 2, //不能使用undefined
    "no-unexpected-multiline": 2, //避免多行表达式
    "no-underscore-dangle": 1, //标识符不能以_开头或结尾
    "no-unneeded-ternary": 2, //禁止不必要的嵌套 var isYes = answer === 1 ? true : false;
    "no-unreachable": 2, //不能有无法执行的代码
    "no-unused-expressions": 2, //禁止无用的表达式
    "no-unused-vars": [2, { "vars": "all", "args": "after-used" }], //不能有声明后未被使用的变量或参数
    "no-use-before-define": 2, //未定义前不能使用
    "no-useless-call": 2, //禁止不必要的call和apply
    "no-void": 2, //禁用void操作符
    "no-var": 0, //禁用var，用let和const代替
    "no-warning-comments": [
      1,
      { "terms": ["todo", "fixme", "xxx"], "location": "start" }
    ], //不能有警告备注
    "no-with": 2, //禁用with
    "array-bracket-spacing": [2, "never"], //是否允许非空数组里面有多余的空格
    "arrow-parens": 0, //箭头函数用小括号括起来
    "arrow-spacing": 0, //=>的前/后括号
    "accessor-pairs": 0, //在对象中使用getter/setter
    "block-scoped-var": 0, //块语句中使用var
    "brace-style": [1, "1tbs"], //大括号风格
    "callback-return": 1, //避免多次调用回调什么的
    "camelcase": 2, //强制驼峰法命名
    "comma-dangle": [2, "never"], //对象字面量项尾不能有逗号
    "comma-spacing": 0, //逗号前后的空格
    "comma-style": [2, "last"], //逗号风格，换行时在行首还是行尾
    "complexity": [0, 11], //循环复杂度
    "computed-property-spacing": [0, "never"], //是否允许计算后的键名什么的
    "consistent-return": 0, //return 后面是否允许省略
    "consistent-this": [2, "that"], //this别名
    "constructor-super": 0, //非派生类不能调用super，派生类必须调用super
    "curly": [2, "all"], //必须使用 if(){} 中的{}
    "default-case": 2, //switch语句最后必须有default
    "dot-location": 0, //对象访问符的位置，换行的时候在行首还是行尾
    "dot-notation": [0, { "allowKeywords": true }], //避免不必要的方括号
    "eol-last": 0, //文件以单一的换行符结束
    "eqeqeq": 2, //必须使用全等
    "func-names": 0, //函数表达式必须有名字
    "func-style": [0, "declaration"], //函数风格，规定只能使用函数声明/函数表达式
    "generator-star-spacing": 0, //生成器函数*的前后空格
    "guard-for-in": 0, //for in循环要用if语句过滤
    "handle-callback-err": 0, //nodejs 处理错误
    "id-length": 0, //变量名长度
    "indent": [2, 4], //缩进风格
    "init-declarations": 0, //声明时必须赋初值
    "key-spacing": [0, { "beforeColon": false, "afterColon": true }], //对象字面量中冒号的前后空格
    "lines-around-comment": 0, //行前/行后备注
    "max-depth": [0, 4], //嵌套块深度
    "max-len": [0, 80, 4], //字符串最大长度
    "max-nested-callbacks": [0, 2], //回调嵌套深度
    "max-params": [0, 3], //函数最多只能有3个参数
    "max-statements": [0, 10], //函数内最多有几个声明
    "new-cap": 2, //函数名首行大写必须使用new方式调用，首行小写必须用不带new方式调用
    "new-parens": 2, //new时必须加小括号
    "newline-after-var": 2, //变量声明后是否需要空一行
    "object-curly-spacing": [0, "never"], //大括号内是否允许不必要的空格
    "object-shorthand": 0, //强制对象字面量缩写语法
    "one-var": 1, //连续声明
    "operator-assignment": [0, "always"], //赋值运算符 += -=什么的
    "operator-linebreak": [2, "after"], //换行时运算符在行尾还是行首
    "padded-blocks": 0, //块语句内行首行尾是否要空行
    "prefer-const": 0, //首选const
    "prefer-spread": 0, //首选展开运算
    "prefer-reflect": 0, //首选Reflect的方法
    "quotes": [1, "single"], //引号类型 `` "" ''
    "quote-props": [2, "always"], //对象字面量中的属性名是否强制双引号
    "radix": 2, //parseInt必须指定第二个参数
    "id-match": 0, //命名检测
    "require-yield": 0, //生成器函数必须有yield
    "semi": [2, "always"], //语句强制分号结尾
    "semi-spacing": [0, { "before": false, "after": true }], //分号前后空格
    "sort-vars": 0, //变量声明时排序
    "space-after-keywords": [0, "always"], //关键字后面是否要空一格
    "space-before-blocks": [0, "always"], //不以新行开始的块{前面要不要有空格
    "space-before-function-paren": [0, "always"], //函数定义时括号前面要不要有空格
    "space-in-parens": [0, "never"], //小括号里面要不要有空格
    "space-infix-ops": 0, //中缀操作符周围要不要有空格
    "space-return-throw-case": 2, //return throw case后面要不要加空格
    "space-unary-ops": [0, { "words": true, "nonwords": false }], //一元运算符的前/后要不要加空格
    "spaced-comment": 0, //注释风格不要有空格什么的
    "strict": 2, //使用严格模式
    "use-isnan": 2, //禁止比较时使用NaN，只能用isNaN()
    "valid-jsdoc": 0, //jsdoc规则
    "valid-typeof": 2, //必须使用合法的typeof的值
    "vars-on-top": 2, //var必须放在作用域顶部
    "wrap-iife": [2, "inside"], //立即执行函数表达式的小括号风格
    "wrap-regex": 0, //正则表达式字面量用小括号包起来
    "yoda": [2, "never"] //禁止尤达条件
  }
}
```

```js
module.exports = {
  root: true,
  parserOptions: {
    parser: "babel-eslint",
    sourceType: "module",
  },
  env: {
    browser: true,
    node: true,
    es6: true,
  },
  extends: ["plugin:vue/recommended", "eslint:recommended"],

  // add your custom rules here
  //it is base on https://github.com/vuejs/eslint-config-vue
  rules: {
    // 强制"for"循环中更新子句的计算器朝着正确的方向移动
    "for-direction": 0,
    // 禁止function定义中出现重名参数
    "no-dupe-args": 2,
    // 禁止对象字面量中出现重复的key
    "no-dupe-keys": 2,
    // 禁止出现重复的case标签
    "no-duplicate-case": 2,
    // 禁止对catch子句的参数重新赋值
    "no-ex-assign": 2,
    // 禁止对关系运算符的左操作数使用否定操作符
    "no-unsafe-negation": 2,
    // 禁止出现令人困惑的多行表达式
    "no-unexpected-multiline": 2,
    // 禁止在return、throw、continue、break语句之后出现不可达代码
    "no-unreachable": 2,
    // 禁止在finally语句块中出现控制流语句
    "no-unsafe-finally": 2,
    // 要求使用isNaN()检查NaN
    "use-isnan": 2,
    // 强制typeof表达式与有效的字符串进行比较
    "valid-typeof": 2,
    // 还可以写表达式，厉害了~
    "no-debugger": process.env.NODE_ENV === "production" ? "error" : "off",
    "no-console": process.env.NODE_ENV === "production" ? "error" : "off",

    /**
     * 【================================================ Best Practices ================================================】
     * 这些规则是关于最佳实践的，帮助你避免一些问题。
     */
    // 强制 getter 和 setter在对象中成对出现
    "accessor-pairs": 2,
    // 强制所有控制语句使用一致的括号风格
    curly: [2, "multi-line"],
    // 强制在点号之前和之后一致的换行
    "dot-location": [2, "property"],
    // 要求使用 ===和 !==
    eqeqeq: [2, "allow-null"],
    // 禁用arguments.caller 或 arguments.callee
    "no-caller": 2,
    // 禁止使用空解构模式
    "no-empty-pattern": 2,
    // 禁止eval()
    "no-eval": 2,
    // 禁止使用类似eval()的方法
    "no-implied-eval": 2,
    // 禁止扩展原生类型
    "no-extend-native": 2,
    // 禁止不必要的.bind()调用
    "no-extra-bind": 2,
    // 禁止case语句落空
    "no-fallthrough": 2,
    // 禁止数字字面量中使用前导和末尾小数点
    "no-floating-decimal": 2,
    // 禁用__iterator__属性
    "no-iterator": 2,
    // 禁用标签语句
    "no-labels": [
      2,
      {
        allowLoop: false,
        allowSwitch: false,
      },
    ],
    // 禁用不必要嵌套块
    "no-lone-blocks": 2,
    // 禁止使用多个空格
    "no-multi-spaces": 2,
    // 禁止使用多行字符串
    "no-multi-str": 2,
    // 禁止对String，Number 和 Boolean 使用new操作符
    "no-new-wrappers": 2,
    // 禁用八进制字面量
    "no-octal": 2,
    // 禁止在字符串中使用八进制转义序列
    "no-octal-escape": 2,
    // 禁止使用__proto__属性
    "no-proto": 2,
    // 禁止多次声明同一变量
    "no-redeclare": 2,
    // 禁止在return语句中使用赋值语句
    "no-return-assign": [2, "except-parens"],
    // 禁止自我赋值
    "no-self-assign": 2,
    // 禁止自我比较
    "no-self-compare": 2,
    // 禁用逗号操作符
    "no-sequences": 2,
    // 禁止抛出异常字面量
    "no-throw-literal": 2,
    // 禁止一成不变的循环条件
    "no-unmodified-loop-condition": 2,
    // 禁止不必要的.call()和.apply()
    "no-useless-call": 2,
    // 禁止不必要的转义字符
    "no-useless-escape": 2,
    // 禁用with语句
    "no-with": 2,
    // 要求IIFE使用括号括起来
    "wrap-iife": 2,
    // 要求或禁止Yoda条件。 if("red" === color) { //字面量在前，变量在后 }
    yoda: [2, "never"], //  比较绝不能是Yoda条件（需要变量在前，字面量在后）

    /**
     * 【================================================ ECMAScript 6 ================================================】
     * 这些规则只与ES6有关，即通常所说的ES2015。
     */
    // 强制箭头函数前后使用一致的空格
    "arrow-spacing": [
      2,
      {
        before: true,
        after: true,
      },
    ],
    // 要求在构造函数中有super()调用
    "constructor-super": 2,
    // 强制generator函数中*号周围使用一致的空格
    "generator-star-spacing": [
      2,
      {
        before: true,
        after: true,
      },
    ],
    // 禁止修改类声明的变量
    "no-class-assign": 2,
    // 禁止修改const声明的变量
    "no-const-assign": 2,
    // 禁止类成员中出现重复的名称
    "no-dupe-class-members": 2,
    // 禁止 Symbolnew 操作符和 new 一起使用
    "no-new-symbol": 2,
    // 禁止在构造函数中，在调用super()之前使用 this 或 super
    "no-this-before-super": 2,
    // 禁止在对象中使用不必要的计算属性
    "no-useless-computed-key": 2,
    // 禁止不必要的构造函数
    "no-useless-constructor": 2,
    // 禁止模板字符串中嵌入表达式周围空格的使用
    "template-curly-spacing": [2, "never"],
    // 强制yield*表达式中的*周围使用空格
    "yield-star-spacing": [2, "both"],
    // 要求使用const声明那些声明后不再被修改的变量
    "prefer-const": 2,

    /**
     * 【================================================ Stylistic Issues ================================================】
     * 这些规则是关于代码风格的。
     */
    // //  获取当前执行环境的上下文时，强制使用一致的命名（此处强制使用 'that'）。
    // 'consistent-this': [2, 'that'],
    // // 禁止或强制在代码块中开括号前和闭括号后有空格  { return 11 }
    // 'block-spacing': [2, 'always'],
    // // 强制在代码块中使用一致的大括号风格
    // 'brace-style': [2, '1tbs', {
    //   'allowSingleLine': true
    // }],
    // // 强制使用驼峰拼写法命名规定
    // 'camelcase': [0, {
    //   'properties': 'always'
    // }],
    // // 要求或禁止末尾逗号
    // 'comma-dangle': [2, 'never'],
    // // 强制在逗号前后使用一致的空格
    // 'comma-spacing': [2, {
    //   'before': false,
    //   'after' : true
    // }],
    // // 强制在逗号前后使用一致的空格
    // 'comma-style': [2, 'last'],
    // // 要求或禁止文件末尾存在空行
    // 'eol-last': 2,
    // // 强制使用一致的缩进
    // 'indent': [2, 2, {
    //   'SwitchCase': 1
    // }],
    // // 强制在JSX属性中一致地使用双引号或单引号
    // 'jsx-quotes': [2, 'prefer-single'],
    // // 要求构造函数首字母大写
    // 'new-cap': [2, {
    //   'newIsCap': true,
    //   'capIsNew': false
    // }],
    // // 要求构造无参构造函数时有圆括号
    // 'new-parens': 2,
    // // 禁用Array构造函数
    // 'no-array-constructor': 2,
    // // 禁止空格和tab的混合缩进
    // 'no-mixed-spaces-and-tabs': 2,
    // // 禁止出现多行空行
    // 'no-multiple-empty-lines': [2, {
    //   // 最大连续空行数
    //   max: 2
    // }],
    // // 禁止在函数标识符和其调用之间有空格
    // 'func-call-spacing': 2,
    // // 禁止行尾空格
    // 'no-trailing-spaces': 2,
    // // 禁止可以在有更简单的可替代的表达式时使用三元操作符
    // 'no-unneeded-ternary': [2, {
    //   // 允许表达式作为默认的赋值模式
    //   'defaultAssignment': true
    // }],
    // // 禁止属性前有空白
    // 'no-whitespace-before-property': 2,
    // // 强制函数中的变量要么一起声明要么分开声明
    // 'one-var': [2, {
    //   'initialized': 'never'
    // }],
    // // 强制操作符使用一致的换行符
    // 'operator-linebreak': [2, 'after', {
    //   'overrides': {
    //     '?': 'before',
    //     ':': 'before'
    //   }
    // }],
    // // 要求或禁止块内填充
    // 'padded-blocks': [2, 'never'],
    // // 强烈使用一致的反勾号``、双引号""或单引号''
    // 'quotes': [2, 'single', {
    //   // 允许字符串使用单引号或者双引号，只要字符串中包含了一个其他引号，否则需要转义
    //   'avoidEscape': true,
    //   // 允许字符串使用反勾号
    //   'allowTemplateLiterals': true
    // }],
    // // 禁止使用分号代替ASI(自动分号插入)
    // 'semi': ["error", "always"],
    // // 强制分号之前和之后使用一致的空格
    // 'semi-spacing': [2, {
    //   'before': false,
    //   'after' : true
    // }],
    // // 强制在块之前使用一致的空格
    // 'space-before-blocks': [2, 'always'],
    // // 强制在圆括号内使用一致的空格
    // 'space-in-parens': [2, 'never'],
    // // 要求操作符周围有空格
    // 'space-infix-ops': 2,
    // // 强制在一元操作符前后使用一致的空格
    // 'space-unary-ops': [2, {
    //   'words'   : true,
    //   'nonwords': false
    // }],
    // // 强制在注释// 或/*使用一致的空格
    // 'spaced-comment': [1, 'always', {
    //   'markers': ['global', 'globals', 'eslint', 'eslint-disable', '*package', '!', ',']
    // }],
    // // 强制在大括号中使用一致的空格
    // 'object-curly-spacing': [2, 'always', {
    //   'objectsInObjects': false
    // }],
    // // 禁止或强制在括号内使用空格
    // 'array-bracket-spacing': [2, 'never'],
    // // 强制要求在对象字面量的属性中键和值之间使用一致的间距
    // 'key-spacing': [2, {
    //   'beforeColon': false,
    //   'afterColon' : true
    // }],

    /**
     * 【================================================ Node.js and CommonJS ================================================】
     * 这些规则是关于Node.js 或 在浏览器中使用CommonJS的。
     */
    // 要求回调函数中有容错处理
    "handle-callback-err": [2, "^(err|error)$"],
    // 禁止调用 require 时使用new操作符
    "no-new-require": 2,
    // 禁止对__dirname和__filename进行字符串连接
    "no-path-concat": 1,
    // 强制在function的左括号之前使用一致的空格
    "space-before-function-paren": [2, "never"],

    /**
     * 【================================================ Possible Errors ================================================】
     * 这些规则与JavaScript代码中可能的错误或逻辑错误有关。
     */
    // 禁止条件表达式中出现赋值操作符
    "no-cond-assign": 2,
    // 禁止在正则表达式中使用控制字符
    "no-control-regex": 0,
    // 禁止在正则表达式中使用空字符集
    "no-empty-character-class": 2,
    // 禁止不必要的布尔转换
    "no-extra-boolean-cast": 2,
    // 禁止不必要的括号
    "no-extra-parens": [2, "functions"],
    // 禁止对function声明重新赋值
    "no-func-assign": 2,
    // 禁止在嵌套块中出现变量声明或function声明
    "no-inner-declarations": [2, "functions"],
    // 禁止RegExp构造函数中存在无效的正则表达式字符串
    "no-invalid-regexp": 2,
    // 禁止在字符串和注释之外不规则的空白
    "no-irregular-whitespace": 2,
    // 禁止把全局对象作为函数调用
    "no-obj-calls": 2,
    // 禁止正则表达式字面量中出现多个空格
    "no-regex-spaces": 2,
    // 禁用稀疏数组
    "no-sparse-arrays": 2,

    /**
     * 【================================================ Variables ================================================】
     * 这些规则与变量声明有关。
     */
    // 禁止删除变量
    "no-delete-var": 2,
    // 不允许标签与变量同名
    "no-label-var": 2,
    // 禁止将标识符定义为受限的名字
    "no-shadow-restricted-names": 2,
    // 禁止未声明的变量，除非它们在/*global */注释中被提到
    "no-undef": 2,
    // 禁止将变量初始化为undefined
    "no-undef-init": 2,
    // 禁止出现未使用的变量
    "no-unused-vars": [
      2,
      {
        var: "all",
        args: "none",
      },
    ],

    /**
     * 【================================================ 配置定义在插件中的规则 ================================================】
     * 格式： 插件名/规则ID
     */
    //
    "vue/no-parsing-error": ["off"],
  },
};
```
