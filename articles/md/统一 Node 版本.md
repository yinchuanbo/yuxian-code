---
title: "统一 Node 版本"
tag: "运行时"
time: 2024-09-01 15:21:24
---

### 1\. package.json 的 engines 字段

在项目的 `package.json` 文件中，可以使用 `engines` 字段来指定所需的 `Node` 版本，在该字段中，可以定义一个范围或者具体的版本号来限制 `Node` 的版本。

```json
// 指定特定版本号
{
  //...
  "engines": {
    "node": "14.17.0"
  }
}
```

```json
// 指定版本号范围
{
  //...
  "engines": {
    "node": ">=12.0.0 <16.0.0"
  }
}
```

```json
// 波浪线符号：表示项目需要 Node 版本为 14.17.x
{
  //...
  "engines": {
    "node": "~14.17.0"
  }
}
```

```json
// 插入符号：表示项目需要 Node 版本为 14.x.x
{
  //...
  "engines": {
    "node": "^14.17.0"
  }
}
```

但是，我们在使用 `npm install` 时，发现 `engines` 配置并没有起作用，然后换 `yarn` 安装，发现 `engines` 配置又起作用了。

**这是怎么回事呢？**

### 2\. 使用 .npmrc 文件

原来 `engines` 只是建议，默认不开启严格版本校验，只会给出提示，需要手动开启严格模式。

在根目录下 `.npmrc` 添加 `engine-strict=true` 才会起作用，配置完成后再执行 `npm install`。

```sh
# .npmrc
engine-strict=true
```

此时，通过 `npm` 安装，限制 `Node` 版本便起作用了。

### 3\. 使用 .nvmrc 文件

通过上面的方式，可以做到让大家使用相同的 Node 版本，但每次提示版本不符合，需要开发人员到 `package.json` 中查看版本号，然后再使用 `nvm` 切换指定版本，太麻烦了，

我们可以创建一个 `.nvmrc` 文件，指定项目 `Node` 版本：

```sh
# .nvmrc
v14.17.5
```

此时，执行 `nvm use` 自动就切换到项目执行的 `Node` 版本。
