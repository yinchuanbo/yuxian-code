---
title: "7 个 Promise 静态方法"
tag: "JavaScript"
time: 2024-09-01 15:21:24
---

Promise 状态变化如下：

![](../imgs/52/01.png)

## Promise.all

Promise.all 方法相信各位都很熟悉，该方法接受一个数组，返回一个 Promise 实例。

Promise.all 的能力如下：

![](../imgs/52/02.png)

- 当所有传入的 promise 都变为成功状态（fulfilled），Promise.all 返回的 promise 也会随着变成完成状态（fulfilled），返回的内容是个数组，数组内容对应传入的各个 promise 的返回。
- 当任意一个传入的 promise 变成了拒绝状态（rejected），Promise.all 返回的 promise 也会立即变成拒绝状态（rejected），返回内容就是第一个拒绝状态的 promise 的返回。

```js
const promise1 = Promise.resolve(18);
const promise2 = 35;
const promise3 = new Promise((resolve, reject) => {
  setTimeout(resolve, 1000, "foo");
});

Promise.all([promise1, promise2, promise3]).then((values) => {
  console.log(values);
});
// 1 秒后输出: Array [18, 25, "foo"]

const promise1 = new Promise((resolve, reject) => {
  setTimeout(reject, 500, "bar");
});
const promise2 = 35;
const promise3 = new Promise((resolve, reject) => {
  setTimeout(resolve, 1000, "foo");
});

Promise.all([promise1, promise2, promise3]).catch((values) => {
  console.log(values);
});
// 0.5s 后输出: bar
```

## Promise.allSettled

Promise.allSettled 该方法接受一个数组，返回一个 Promise 实例。

Promise.allSettled 的能力如下：

![](../imgs/52/01.webp)

当所有传入的 promise 都变为已完成状态（settled ），无论它们成功还是失败，Promise.allSettled 会返回一个 promise 实例，状态为已成功，参数是个数组，数组的对象都有三个属性：

- status：fulfilled 或者 rejected，代表 promise 的状态
- value：fulfilled 时才存在，为 promise 返回的值
- reason：rejected 时才存在，为 promise 返回的值

```js
const promise1 = Promise.resolve(3);
const promise2 = new Promise((resolve, reject) =>
  setTimeout(reject, 1000, "foo")
);
const promises = [promise1, promise2];

Promise.allSettled(promises).then((results) =>
  results.forEach((result) =>
    console.log(result.status === "fulfilled" ? result.value : result.reason)
  )
);
// 1s 后依次输出：3、foo
```

**Promise.allSettled vs Promise.all**

- Promise.all 需要所有任务都返回成功才行，只有一个失败，就立即返回。比如一系列规则校验，全部通过才是校验成功，只要一个失败就立即返回失败。
- Promise.allSettled 更关注一系列任务是否都完成（settled）了，同时无论任务成功（fulfilled）还是失败（rejected），每个任务的返回都能在最后的结果中查看。比如我上传三个文件，上传结果在所有任务都返回后再给出（如：上传成功两个，失败一个），同时能对失败的上传做额外处理。

## Promise.any

Promise.any 该方法同样接受一个数组，返回一个 Promise 实例。

Promise.any 的能力如下：

![](../imgs/52/02.webp)

- 当传入的 promise 有一个变成成功状态（fulfilled），Promise.any 返回的 promise 也会随着变成完成状态（fulfilled），返回的内容是最先 resolve 的 promise 的返回。
- 当所有传入的 promise 变成了拒绝状态（rejected），Promise.any 返回的 promise 也会变成拒绝状态（rejected），返回的内容是个对象，对象的 errors 属性对应传入的各个 promise 的返回。

```js
const promise1 = Promise.reject(0);
const promise2 = new Promise((resolve) => setTimeout(resolve, 100, "quick"));
const promise3 = new Promise((resolve) => setTimeout(resolve, 500, "slow"));

const promises = [promise1, promise2, promise3];

Promise.any(promises).then((value) => console.log(value));
// 0.1s 后输出：quick

const promise1 = Promise.reject(0);
const promise2 = new Promise((resolve, reject) =>
  setTimeout(reject, 100, "quick")
);
const promise3 = new Promise((resolve, reject) =>
  setTimeout(reject, 500, "slow")
);

const promises = [promise1, promise2, promise3];

Promise.any(promises).catch((value) => {
  console.dir(value.errors);
});
// 0.5s 后输出：[0, 'quick', 'slow']
```

**Promise.any vs Promise.all**

Promise.any 和 Promise.all 可以与 Array.prototype.some() 和 Array.prototype.every() 进行类比。

Promise.any 更关注某个最先成功的任务，比如你向多个服务器节点请求同个资源，哪个最先返回就使用哪个。

## Promise.race

Promise.race 该方法同样接受一个数组，返回一个 Promise 实例。

Promise.race 的能力如下：

![](../imgs/52/03.png)

当传入的 promise 数组中有一个状态为已完成（settled）, Promise.race 返回的 promise 就会根据最先完成的 promise 改变状态（fulfilled 或 rejected），并返回内容。

```js
const promise1 = new Promise((resolve, reject) => {
  setTimeout(resolve, 500, "one");
});

const promise2 = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, "two");
});

Promise.race([promise1, promise2]).then((value) => {
  console.log(value);
});
// 0.1s 后输出: "two"

const promise1 = new Promise((resolve, reject) => {
  setTimeout(resolve, 500, "one");
});

const promise2 = new Promise((resolve, reject) => {
  setTimeout(reject, 100, "two");
});

Promise.race([promise1, promise2]).catch((value) => {
  console.log(value);
});
// 0.1s 后输出: "two"
```

**Promise.race vs Promise.any**

- Promise.race 更关注第一个返回的，无论状态是成功还是失败。
- Promise.any 则是关注第一个成功返回的。

Promise.race 可以用来控制请求并发，代码如下：

```js
/**
 * poolLimit: 并发数
 * iterable: 并发迭代数组
 * iteratorFn: 并发执行函数
 */
async function asyncPool(poolLimit, iterable, iteratorFn) {
  // 用于保存所有异步请求
  const ret = [];
  // 用户保存正在进行的请求
  const executing = new Set();
  for (const item of iterable) {
    // 构造出请求 Promise
    const p = Promise.resolve().then(() => iteratorFn(item, iterable));
    ret.push(p);
    executing.add(p);
    // 请求执行结束后从正在进行的数组中移除
    const clean = () => executing.delete(p);
    p.then(clean).catch(clean);
    // 如果正在执行的请求数大于并发数，就使用 Promise.race 等待一个最快执行完的请求
    if (executing.size >= poolLimit) {
      await Promise.race(executing);
    }
  }
  // 返回所有结果
  return Promise.all(ret);
}

// 使用方法
const timeout = (i) =>
  new Promise((resolve) => setTimeout(() => resolve(i), i));
asyncPool(2, [1000, 5000, 3000, 2000], timeout).then((results) => {
  console.log(results);
});

// 6s 后输出：[1000, 5000, 3000, 2000]
```

核心实现就是使用 Promise.race 和 Promise.all。如果发出的请求大等于并发数了，这时就不会发起下一个请求，而是使用 Promise.race 等待一个最快执行结束的请求，然后在继续发请求。最后通过 Promise.all 返回所有结束。

## Promise.resolve

Promise.resolve 这个相信大家都有在平时开发中使用，用于快速创建一个状态为 fulfilled 的 promise 实例。

```js
const promise1 = Promise.resolve(123);

promise1.then((value) => {
  console.log(value);
});

// 输出: 123
```

## Promise.reject

Promise.reject 类似 Promise.resolve，用于快速创建一个状态为 rejected 的 promise 实例。

```js
function resolved(result) {
  console.log("Resolved");
}

function rejected(result) {
  console.log(result);
}

Promise.reject(new Error("fail")).then(resolved, rejected);

// 输出: fail
```

## Promise.withResolvers

当我们需要把回调函数式的异步编程兼容成 Promise 时，我们通常会用 `new Promise((resolve, rejcet) => {})` 来处理，有了 `Promise.withResolvers` 后，代码会变得更简单。

```js
const { promise, resolve, reject } = Promise.withResolvers();

// 等同于
let resolve, reject;
const promise = new Promise((res, rej) => {
  resolve = res;
  reject = rej;
});
```

异步编程可以根据自己喜欢选择编写方式：

```js
const { promise, resolve, reject } = Promise.withResolvers();
setTimeout(resolve, 1000, "done");
promise.then(console.log);
// 1s 后输出: done

// 等同于
const promise = new Promise((resolve, reject) => {
  setTimeout(resolve, 1000, "done");
});
promise.then(console.log);
```
