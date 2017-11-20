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

导入的`webpack`函数需要传入一个 webpack[配置对象](//configuration/README.md)，_**同时**_传入回调函数时就会执行 webpack 编译器：

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
> 编译错误**不**在`err`对象内，而是需要使用`stats.hasErrors()`单独处理，你可以在指南的[错误处理](#错误处理error-handling)部分查阅到更多细节。`err`对象只会包含 webpack 相关的问题，比如配置错误等。

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

如果你**不**向`webpack()`函数传入回调函数，就会得到（`webpack()`执行后返回）一个 webpack`Compiler`实例。你可以通过它手动触发 webpack 执行器，或者是让它执行构建并监听变更。和 [CLI](///API/CLI.md) API 很类似。`Compiler`实例提供了以下方法:

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

### 作废`Watching`\(Invalidate`Watching`\)

使用`watching.invalidate`，您可以手动使当前编译循环\(compiling round\)无效，而不会停止监视进程：

```js
watching.invalidate();
```

## Stats 对象\(Stats Object\)

`stats`对象会被作为[`webpack()`](#webpack)回调函数的第二个参数传入，可以通过它获取到代码编译过程中的有用信息，包括：

* 错误和警告（如果有的话）
* 计时信息
* module 和 chunk 信息

[webpack CLI](///API/CLI.md)正是基于这些信息在控制台展示友好的格式输出。

> **\[info\]** 注：
>
> 当使用[`MultiCompiler`](https://doc.webpack-china.org/api/plugins/compiler#multicompiler)时，一个`MultiStats`实例被返回，这个实例实现与`stats`相同的接口，也就是下面描述的方法。

`stats`对象暴露了以下方法：

### `stats.hasErrors()`

可以用来检查编译期是否有错误，返回`true`或`false`。

### `stats.hasWarnings()`

可以用来检查编译期是否有警告，返回`true`或`false`。

### `stats.toJson(options)`

以 JSON 对象形式返回编译信息。`options`可以是一个字符串（预设值）或是颗粒化控制的对象：

```js
stats.toJson("minimal"); // 更多选项如: "verbose" 等.
```

```js
stats.toJson({
  assets: false,
  hash: true
});
```

所有可用的配置选项和预设值都可查询[Stats 文档。](///configuration/stats.md)

> 这里有[一个该函数输出的示例](https://github.com/webpack/analyse/blob/master/app/pages/upload/example.json)

### `stats.toString(options)`

以格式化的字符串形式返回描述编译信息（类似[CLI](///API/CLI.md)的输出）。

配置对象与[`stats.toJson(options)`](#statstojsonoptions)一致，除了额外增加的一个选项：

```js
stats.toString({
  // ...
  // 增加控制台颜色开关
  colors: true
});
```

下面是`stats.toString()`用法的示例：

```js
const webpack = require("webpack");

webpack({
  // 配置对象
}, (err, stats) => {
  if (err) {
    console.error(err);
    return;
  }

  console.log(stats.toString({
    chunks: false,  // 使构建过程更静默无输出
    colors: true    // 在控制台展示颜色
  }));
});

```

## 错误处理\(Error Handling\)

完备的错误处理中需要考虑以下三种类型的错误：

* 致命的 wepback 错误（配置出错等）
* 编译错误（缺失的 module，语法错误等）
* 编译警告

下面是一个覆盖这些场景的示例：

```js
const webpack = require("webpack");

webpack({
  // 配置对象
}, (err, stats) => {
  if (err) {
    console.error(err.stack || err);
    if (err.details) {
      console.error(err.details);
      // webpack 相关的错误，比如配置错误
    }
    return;
  }

  const info = stats.toJson();

  if (stats.hasErrors()) {
    console.error(info.errors);
    // 编译错误
  }

  if (stats.hasWarnings()) {
    console.warn(info.warnings);
    // 编译警告
  }

  // 记录结果...
});
```

## 自定义文件系统\(Custom File Systems\)

默认情况下，webpack 使用普通文件系统来读取文件并将文件写入磁盘。但是，还可以使用不同类型的文件系统（内存\(memory\), webDAV 等）来更改输入或输出行为。为了实现这一点，可以改变`inputFileSystem`或`outputFileSystem`。例如，可以使用[`memory-fs`](https://github.com/webpack/memory-fs)替换默认的`outputFileSystem`，以将文件写入到内存中，而不是写入到磁盘：

```js
const MemoryFS = require("memory-fs");
const webpack = require("webpack");

const fs = new MemoryFS();
const compiler = webpack({ /* options*/ });

compiler.outputFileSystem = fs;
compiler.run((err, stats) => {
  // 之后读取输出：
  const content = fs.readFileSync("...");
});
```

值得一提的是，被[webpack-dev-server](https://github.com/webpack/webpack-dev-server)及众多其他包依赖的[webpack-dev-middleware](https://github.com/webpack/webpack-dev-middleware)就是通过这种方式，将你的文件神秘地隐藏起来，但却仍然可以用它们为浏览器提供服务！

> **\[info\]** 注：
>
> 你指定的输出文件系统需要兼容 Node 自身的[`fs`](https://nodejs.org/api/fs.html)模块接口，接口需要提供`mkdirp`和`join`工具方法。



