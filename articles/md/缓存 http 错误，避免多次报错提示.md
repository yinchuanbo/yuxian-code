---
title: "缓存 http 错误，避免多次报错提示"
tag: "JavaScript"
time: 2024-09-01 15:21:24
---

### 前言

先解释一下标题，最近在开发一个老项目时发现一个非常影响用户体验的场景：

![](../imgs/38/01.awebp)

因为项目性质问题，我们给项目设置了较短的登录有效时间，而当登录过期时用户在某一页面进行操作，如果该操作同时请求了多个请求，则会得到多条登录过期的报错提示。作为一个专业的前端，怎么可以允许这种情况发生呢，于是就有了今天文章的内容。

### 缓存

项目中使用的 http 为统一二次封装的请求函数，登录过期的错误也是统一进行处理：

```js
service.interceptors.response.use(
  (respon) => {
    xxxxx;
  },
  (error) => {
    let status = JSON.parse(JSON.stringify(error.response));
    //跳转失败,返回首页.
    if (status.data.code == "401") {
      Message({
        message: status.data.message,
        type: "error",
        duration: 5 * 1000,
      });
      router.push({
        path: "/login",
      });
    }
    return Promise.reject(error);
  }
);
```

如上，当接口返回`401`时，则表明接口登录失效，需要抛出`message`,这个过程中，如果同时有几个接口返回`401`则会有多少条`message`。这时，我们可以有什么办法可以处理同一类型的错误只报错一次呢？没错，这就是我们今天要讲的方法：缓存。

首先，我们需要明确自己的需求：

- 根据 http 错误码进行缓存，以达到判断同种的 http 错误码的错误
- 缓存具有过期机制，当过了设置的有效期自动清除缓存的错误

上代码！！！！！！

```js
// errorCache.js
// config = {
//  cacheTime: 'xxxx'
// }
const errorCache = {};

function isCacheError(error, config) {
  if (
    errorCache[error.code] &&
    !isExpire(errorCache[error.code].timestamp, config.cacheTime)
  ) {
    return true;
  }

  errorCache[error.code] = {
    timestamp: Date.now(),
  };
  setTimeout(() => {
    delete errorCache[error.code];
  }, config.cacheTime);

  return false;
}

function isExpire(timestamp, cacheTime) {
  return Date.now() - timestamp > cacheTime;
}

export default isCacheError;
```

思路：

- 声明缓存用对象`errorCache`,`key`为 http 错误码，`value`为错误的时间戳。例：`{401: {timestamp: 1686122336413}}`
- 每类错误都将在缓存在`errorCache`中，在设置的`config.cacheTime`内，同类错误都将返回`true`,反之则将错误添加至缓存并返回`false`

应用：

```js
(error) => {
  let status = JSON.parse(JSON.stringify(error.response));
  if (!isCacheError(status.data, { cacheTime: 5 * 1000 })) {
    if (status.data.code == "401") {
      Message({
        message: status.data.message,
        type: "error",
        duration: 5 * 1000,
      });
      router.push({
        path: "/login",
      });
    }
  }
  return Promise.reject(error);
};
```
