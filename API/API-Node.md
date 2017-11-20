# Node.js API

webpack 提供了 Node.js API，可以在 Node.js 运行时下直接使用。

当你需要自定义构建或开发流程时，Node.js API 非常有用，因为此时所有的报告和错误处理都必须自行实现，webpack 仅仅负责编译的部分。所以[`stats`](///configuration/stats.md)配置选项不会在`webpack()`调用中生效。

## 安装\(Installation\)

开始使用 webpack 的 Node.js API 之前，首先你需要安装 webpack：

```bash
npm install --save-dev webpack
```

然后在 Node.js 脚本中`require`webpack 模块：

```js
const webpack = require("webpack");

// 或者如果你喜欢 ES2015:
import webpack from "webpack";
```

## `webpack()`

导入的`webpack`函数需要传入一个 webpack[配置对象](//configuration/README.md)，当同时传入回调函数时就会执行 webpack 编译器：

```js
const webpack = require("webpack");

webpack({
  // 配置对象
}, (err, stats) => {
  if (err || stats.hasErrors()) {
    // 在这里处理错误
  }
  // 处理完成
});
```

> **\[info\]** 注：
>
> 编译错误**不**在`err`对象内，而是需要使用`stats.hasErrors()`单独处理，你可以在指南的[错误处理](https://doc.webpack-china.org/api/node/#-error-handling-)部分查阅到更多细节。`err`对象只会包含 webpack 相关的问题，比如配置错误等。

你还可以传入一个配置选项**数组**到`webpack`函数内：

```js
webpack([
  { /* 配置对象 */ },
  { /* 配置对象 */ },
  { /* 配置对象 */ }
], (err, stats) => {
  // ...
});
```

> **\[info\]** 注：
>
> webpack**不**会并行执行多个配置。每个配置只会在前一个处理结束后才会开始处理。如果你需要 webpack 并行执行它们，你可以使用像[parallel-webpack](https://www.npmjs.com/package/parallel-webpack)这样的第三方解决方案。

## Compiler 实例\(Compiler Instance\)

如果你不向`webpack`执行函数传入回调函数，就会得到（webpack执行后返回）一个 webpack`Compiler`实例。你可以通过它手动触发 webpack 执行器，或者是让它执行构建并监听变更。和 [CLI](///API/CLI.md) API 很类似。`Compiler`实例提供了以下方法:

* `.run(callback)`

* `.watch(watchOptions, handler)`

> **\[warning\] 注：**
>
> API只支持一次并发编译。在使用`run`时，在调用`run`或`watch`之前等待它完成。当使用`watch`时，调用`close`，等待它完成，然后再一次调用`run`或`watch`。并发编译会损坏输出文件。

### 执行\(Run\)

调用`Compiler`实例的`run`方法跟上文提到的快速执行方法很相似：

```js
const webpack = require("webpack");

const compiler = webpack({
  // 配置对象
});

compiler.run((err, stats) => {
  // ...
});
```

### 监听\(Watching\)

调用`watch`方法会触发 webpack 执行器，但之后会监听变更（很像 CLI 命令:`webpack --watch`），一旦 webpack 检测到文件变更，就会重新执行编译。该方法返回一个`Watching`实例。

```js
watch(watchOptions, callback)
```

```js
const webpack = require("webpack");

const compiler = webpack({
  // 配置对象
});

const watching = compiler.watch({
  /* watchOptions */
}, (err, stats) => {
  // 在这里打印 watch/build 结果...
  console.log(stats);
});
```

`Watching`配置选项的[细节可以在这里查阅](/configuration/watchOptions.md)。

### 关闭`Watching`\(Close`Watching`\)

`watch`方法返回一个`Watching`实例，它会暴露一个`.close(callback)`方法。调用该方法将会结束监听：

```js
watching.close(() => {
  console.log("Watching Ended.");
});
```

> **\[info\]** 注：
>
> 不允许在当前监听器已经关闭或失效前再次监听或执行。



