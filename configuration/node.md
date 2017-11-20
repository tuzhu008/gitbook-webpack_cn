# Node

这些选项将配置是否填充（polyfill）或模拟某个[Node.js 全局变量](#)和模块。这允许最初为Node.js 环境编写代码，以来在其他环境中运行，比如浏览器。

这个特性是由webpack内部的[`NodeStuffPlugin`](https://github.com/webpack/webpack/blob/master/lib/NodeStuffPlugin.js)提供的。如果目标是“web”\(默认\)或“webworker”，[`NodeSourcePlugin`](https://github.com/webpack/webpack/blob/master/lib/node/NodeSourcePlugin.js)插件也会被激活。

## `node`

类型：object

这是一个对象，其中每个属性都是一个Node全局变量或模块的名称，每个值可能是下面的一个。

* `true`:  提供一个polyfill。
* `"mock"`: 提供一个实现预期接口的mock，但它几乎没有或没有功能。
* `"empty"`: 提供一个空对象。
* `false`: 不提供任何东西。预期该对象的代码可能会产生`ReferenceError而`崩溃。试图使用`require('modulename')`导入模块的代码可能触发一个`Cannot find module "modulename"`错误。

> **\[warning\]** 注：
>
> 并不是所有的Node 全局变量都支持这四个选项。编译器将为不受支持的属性值组合抛出一个错误\(例如:`process: 'empty'`\)。更多细节请参见下面的部分。

这些是默认值:

```js
node: {
  console: false,
  global: true,
  process: true,
  __filename: "mock",
  __dirname: "mock",
  Buffer: true,
  setImmediate: true

  // 请参阅“其他 node 核心库”以获得更多选项。
}
```

由于webpack 3.0.0，`node`选项可能被设置为false，以完全关闭`NodeStuffPlugin`和`NodeSourcePlugin`插件。

### `node.console`

类型：boolean \| "mock"

默认值: `false`

浏览器提供了一个与Node.js `console`非常相似的`console`对象，所以通常不需要 polyfill。

### `node.process`

类型：boolean \| "mock"

默认值: `true`

### `node.global`

类型：boolean

默认: `true`

查看该对象的确切行为的[源码](https://github.com/webpack/webpack/blob/master/buildin/global.js)。

### `node.__filename`

类型：boolean \| "mock"

默认值: `"mock"`

Options:

* `true`: 相对于[`context` 选项](https://webpack.js.org/configuration/entry-context/#context) 的输入文件的文件名。
* `false`: The regular Node.js `__filename` behavior. The filename of the **output** file when run in a Node.js environment.普通的Node.js ` __filename`行为。在 Node.js 环境中运行时输出文件的文件名。
* `"mock"`: 固定值`“index.js”`。

### `node.__dirname`

类型：boolean \| "mock"

默认: `"mock"`

Options:

* `true`: 相对于[`context` 选项](https://webpack.js.org/configuration/entry-context/#context) 的输入文件的目录名称 （dirname）。
* `false`: 普通的Node.js ` __dirname`行为。在 Node.js 环境中运行时输出文件的dirname。
* `"mock"`: 固定值" / "。

### `node.Buffer`

类型：boolean \| "mock"

默认: `true`

### `node.setImmediate`

类型：boolean \| "mock" \| "empty"

默认: `true`

## 其他 node 核心库

类型：boolean \| "mock" \| "empty"

> **\[warning\]** 注：
>
> 这个选项只有在目标未指明的情况下才会被激活\(通过`NodeSourcePlugin`\)，“web”或“webworker”。

当`NodeSourcePlugin`插件启用时，如果可以，来自[`node-libs-browser`](https://github.com/webpack/node-libs-browser)的 Node.js 核心库的Polyfills被使用。查看[Node.js 核心库和他们的polyfills](https://github.com/webpack/node-libs-browser#readme)。

在默认情况下，webpack 将会在每个库中进行polyfills，如果有一个已知的polyfill，或者什么都不做的话。在后一种情况下，webpack会表现得好像模块名配置了`false`值。

为了导入一个内置模块，使用非webpack请求，即非webpack请求\(“模块化”\)而不是要求\(“模块化”\)。

W&gt; This option is only activated \(via `NodeSourcePlugin`\) when the target is unspecified, "web" or "webworker".

Polyfills for Node.js core libraries from [`node-libs-browser`](https://github.com/webpack/node-libs-browser) are used if available, when the `NodeSourcePlugin` plugin is enabled. See the list of [Node.js core libraries and their polyfills](https://github.com/webpack/node-libs-browser#readme).

By default, webpack will polyfill each library if there is a known polyfill or do nothing if there is not one. In the latter case, webpack will behave as if the module name was configured with the `false` value.

T&gt; To import a built-in module, use [`__non_webpack_require__`](/api/module-variables/#__non_webpack_require__-webpack-specific-), i.e. `__non_webpack_require__('modulename')` instead of `require('modulename')`.

Example:

```js
node: {
  dns: "mock",
  fs: "empty",
  path: true,
  url: false
}
```



