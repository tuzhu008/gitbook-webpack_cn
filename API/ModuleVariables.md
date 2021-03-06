# Module Variables （模块变量）

这个章节涵盖了使用webpack编译的代码中可用的所有**变量**（**variables**）。模块可以通过`module`和其他变量访问来自编译过程的某些数据。

> **\[info\]** 注：
>
> 本节中的括号中的环境代表这个变量的原生环境。
>
> **webpack-specific：**webpack自有的
>
> **NodeJS：**Node.js中的
>
> **CommonJS：**CommonJS

### `module.loaded` \(NodeJS\)

类型：boolean

* `false` 表示该模块正在执行
* `true` 表示同步执行已经完成。

### `module.hot` \(webpack-specific\)

表示 [热模块替换 \(Hot Module Replacement\)](/concepts/hot-module-replacement) 是否启用，并给进程提供一个接口。详细说明见 [热模块替换 API 页面](//API/HMR.md)

### `module.id` \(CommonJS\)

该模块的 id 。

```js
module.id === require.resolve("./file.js")
```

### `module.exports` \(CommonJS\)

类型：object  \| function

当用户 `require` 了该模块后的返回值 \(默认为一个新对象\)

```js
module.exports = function doSomething() {
  // Do something...
};
```

> **\[warning\] 注：**
>
> 无法在异步函数中使用该功能

### `exports` \(CommonJS\)

该变量默认值为 `module.exports`（即一个对象）。 如果 `module.exports` 被重写的话， `exports` 不再会被导出。

```javascript
exports.someValue = 42;
exports.anObject = {
    x: 123
};
exports.aFunction = function doSomething() {
    // Do something
};
```

### `global` \(NodeJS\)

见 [node.js global](http://nodejs.org/api/globals.html#globals_global).

### `process` \(NodeJS\)

见 [node.js process](http://nodejs.org/api/process.html).

### `__dirname` \(NodeJS\)

取决于 `node.__dirname` 的配置选项：

* `false`: Not defined
* `mock`: 等于 "/"
* `true`: [node.js \_\_dirname](http://nodejs.org/api/globals.html#globals_dirname)

如果在一个被解析的表达式内部使用，则配置选项会被当作 `true` 处理。

### `__filename` \(NodeJS\)

取决于 `node.__filename` 的配置选项：

* `false`: Not defined
* `mock`: 等于 "/index.js"
* `true`: [node.js \_\_filename](http://nodejs.org/api/globals.html#globals_filename)

如果在一个被解析的表达式内部使用，则配置选项会被当作 `true` 处理。

### `__resourceQuery` \(webpack-specific\)

当前模块的资源查询 \(resource query\) 。如果之后有对该模块的 `reqiure` ，那么查询字符串 \(query string\) 会在 `file.js` 中可访问。

```javascript
require('file.js?test')
```

**file.js**

```javascript
__resourceQuery === '?test'
```

### `__webpack_public_path__` \(webpack-specific\)

等同于配置选项 `output.publicPath`.

### `__webpack_require__` \(webpack-specific\)

webpack特有的require 函数。 这个表达式不会由于依赖而被解析器解析。

### `__webpack_chunk_load__` \(webpack-specific\)

内部 chunk 载入函数，有两个输入参数：

* `chunkId` 需要载入的块的id
* `callback(require)` 块载入后的回调函数

### `__webpack_modules__` \(webpack-specific\)

访问所有模块的内部对象。

### `__webpack_hash__` \(webpack-specific\)

这个变量只有在启用 `HotModuleReplacementPlugin` 或者 `ExtendedAPIPlugin` 时才生效。 这个变量提供了编译过程中\(compilation\)的 hash 信息的获取。

### `__non_webpack_require__` \(webpack-specific\)

生成一个不会被 webpack  解析的 `require` 函数。 在可能的情况下配合全局 require 函数可以完成一些酷炫操作。

### `DEBUG`  \(webpack-specific\)

等同于配置参数中的 `debug`



