# Webpack Manifest Plugin [![Build Status](https://travis-ci.org/danethurber/webpack-manifest-plugin.svg?branch=master)](https://travis-ci.org/danethurber/webpack-manifest-plugin)  [![codecov](https://codecov.io/gh/danethurber/webpack-manifest-plugin/badge.svg?branch=master)](https://codecov.io/gh/danethurber/webpack-manifest-plugin?branch=master) [![Gitter](https://img.shields.io/gitter/room/nwjs/nw.js.svg)](https://gitter.im/webpack-manifest-plugin#)


用来生成资产清单的Webpack plugin。
> 注意: 下面是关于`webpack-manifest-plugin`的下一个主要版本， 请查看 https://github.com/danethurber/webpack-manifest-plugin/blob/1.x/README.md 获取 `v1` 文档。

## 安装

```bash
npm install --save-dev webpack-manifest-plugin
```

## 用法

在你的 `webpack.config.js`文件中

```javascript
var ManifestPlugin = require('webpack-manifest-plugin');

module.exports = {
    // ...
    plugins: [
      new ManifestPlugin()
    ]
};
```

这将生成一个`manifest.json`文件在你的根输出目录，文件记录了所有源文件名映射到对应的输出文件的信息，例如：


```json
{
  "mods/alpha.js": "mods/alpha.1234567890.js",
  "mods/omega.js": "mods/omega.0987654321.js"
}
```


## API:

```js
// webpack.config.js

module.exports = {
  output: {
    publicPath
  },
  plugins: [
    new ManifestPlugin(options)
  ]
}
```

### `publicPath`

类型: `String`

`publicPath`是一个路径前缀，将被添加到manifest的值中。

### `options.fileName`

类型: `String`<br>
默认值: `manifest.json`

在输出目录的manifest文件名。


### `options.basePath`

类型: `String`


所有键的路径前缀。用于在manifest中包括输出路径。


### `options.writeToFileEmit`

类型: `Boolean`<br>
默认值: `false`

如果设置为`true`，将与`webpack-dev-server`结合，一起（emit）构建文件夹和内存


### `options.seed`

类型: `Object`<br>
默认值: `{}`

  A cache of key/value pairs to used to seed the manifest. This may include a set of [custom key/value](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/manifest.json) pairs to include in your manifest, or may be used to combine manifests across compilations in [multi-compiler mode](https://github.com/webpack/webpack/tree/master/examples/multi-compiler). To combine manifests, pass a shared seed object to each compiler's ManifestPlugin instance.

用于对manifest进seed的键/值对的缓存。这可能包括在您的清单中包含的一组[自定义键/值对](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/manifest.json)，或者可以用于在[多编译器模式](https://github.com/webpack/webpack/tree/master/examples/multi-compiler)中跨编译组合显示。为了合并显示，将一个共享的种子对象传递给每个编译器的manifest插件实例。
### `options.filter`

Type: `function`

Filter out files. [more details](#hooks-options)


### `options.map`

Type: `function`

Modify files details before the manifest is created. [more details](#hooks-options)

### `options.sort`

Type: `function`

Sort files before they are passed to `generate`. [more details](#hooks-options)

### `options.generate`

Type: `function`<br>
Default: `(seed, files) => files.reduce((manifest, {name, path}) => ({...manifest, [name]: path}), seed)`

All entries in `files` correspond to the object structure described in the `Hooks Options` section.

Create the manifest. It can return anything as long as it's serialisable by `JSON.stringify`. [more details](#hooks-options)


## Hooks Options

`filter`, `map`, `sort` takes as an input an Object with the following properties:

### `path`

Type: `String`


### `chunk`

Type: [`Chunk`](https://github.com/webpack/webpack/blob/master/lib/Chunk.js)


### `name`

Type: `String`, `null`


### `isChunk`

Type: `Boolean`


### `isInitial`

Type: `Boolean`

Is required to run you app. Cannot be `true` if `isChunk` is `false`.


### `isAsset`

Type: `Boolean`


### `isModuleAsset`

Type: `Boolean`

Is required by a module. Cannot be `true` if `isAsset` is `false`.


## License

MIT © [Dane Thurber](https://github.com/danethurber)




