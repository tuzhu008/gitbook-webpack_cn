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

W&gt; Not every Node global supports all four options. The compiler will throw an error for property-value combinations that aren't supported \(e.g. `process: 'empty'`\). See the sections below for more details.并不是所有的节点都支持这四个选项。编译器将为不受支持的属性值组合抛出一个错误\(例如:进程:“pty”\)。更多细节请参见下面的部分。

These are the defaults:这些是默认值:

```js
node: {
  console: false,
  global: true,
  process: true,
  __filename: "mock",
  __dirname: "mock",
  Buffer: true,
  setImmediate: true

  // See "Other node core libraries" for additional options.
}
```

Since webpack 3.0.0, the `node` option may be set to `false` to completely turn off the `NodeStuffPlugin` and `NodeSourcePlugin` plugins.由于webpack 3.0.0，节点选项可能被设置为false，以完全关闭NodeStuffPlugin和NodeSourcePlugin插件。

## `node.console`

`boolean | "mock"`

Default: `false`

The browser provides a `console` object with a very similar interface to the Node.js `console`, so a polyfill is generally not needed.浏览器提供了一个与节点非常相似的控制台对象。js控制台，所以通常不需要多填充。

## `node.process`

`boolean | "mock"`

Default: `true`

## `node.global`

`boolean`

Default: `true`

See [the source](https://github.com/webpack/webpack/blob/master/buildin/global.js) for the exact behavior of this object.查看该对象的确切行为的源。

## `node.__filename`

`boolean | "mock"`

Default: `"mock"`

Options:

* `true`: The filename of the **input** file relative to the [`context` option](https://webpack.js.org/configuration/entry-context/#context).相对于上下文选项的输入文件的文件名。
* `false`: The regular Node.js `__filename` behavior. The filename of the **output** file when run in a Node.js environment.普通节点。js \_\_filename行为。在节点中运行时输出文件的文件名。js的环境。
* `"mock"`: The fixed value `"index.js"`.固定值“index.js”。

## `node.__dirname`

`boolean | "mock"`

Default: `"mock"`

Options:

* `true`: The dirname of the **input** file relative to the [`context` option](https://webpack.js.org/configuration/entry-context/#context).相对于上下文选项的输入文件的dirname。
* `false`: The regular Node.js `__dirname` behavior. The dirname of the **output** file when run in a Node.js environment.普通节点。js \_\_dirname行为。在节点中运行时输出文件的dirname。js的环境。
* `"mock"`: The fixed value `"/"`.固定值" / "。

## `node.Buffer`

`boolean | "mock"`

Default: `true`

## `node.setImmediate`

`boolean | "mock" | "empty"`

Default: `true`

## Other node core libraries

`boolean | "mock" | "empty"`

W

这个选项只有在目标未指明的情况下才会被激活\(通过NodeSourcePlugin\)，“web”或“webworker”。

Polyfills节点。当NodeSourcePlugin插件启用时，可以使用node-libs浏览器的js核心库。查看节点列表。js的核心库和它们的多填充。

在默认情况下，webpack将会在每个库中进行多填充，如果有一个已知的多填充，或者什么都不做的话。在后一种情况下，webpack会表现得好像模块名配置了假值。

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



