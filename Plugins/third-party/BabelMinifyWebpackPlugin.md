# BabelMinifyWebpackPlugin

一个用于[babel-minify](https://github.com/babel/minify)的 webpack 插件 - 基于 babel 的 minifier

## 安装

```bash
npm install babel-minify-webpack-plugin --save-dev
```

## 用法

```js
// webpack.config.js
const MinifyPlugin = require("babel-minify-webpack-plugin");
module.exports = {
  entry: //...,
  output: //...,
  plugins: [
    new MinifyPlugin(minifyOpts, pluginOpts)
  ]
}
```

## 选项

### `minifyOpts`

类型：object

默认值：`{}`

`minifyOpts`被传递给 babel-preset-minify。 你可以在包目录中找到[所有可用的选项](https://github.com/babel/minify/tree/master/packages/babel-preset-minify#options)。

### `pluginOpts`

类型：object

`pluginOpts`有以下属性：

* `test`: JS文件扩展名正则表达式。 默认:`/\.js($|\?)/i`

* `comments`: 保留注释。 默认:`/^\**!|@preserve|@license|@cc_on/`,`falsy`值将移除所有注释。可以接受函数，带有测试属性的（正则）的对象和值。

* `sourceMap`: 默认: 使用[webpackConfig.devtool](//configuration/devtools.md)。 这里的设置会覆写`devtool`的设置。
* `parserOpts`: 配置具有特殊解析器选项的babel。
* `babel`: 传入一个自定义的 babel-core，代替原来的。`require("babel-core")`
* `minifyPreset`: 传入一个自定义的 minify preset，代替原来的。 -`require("babel-preset-minify")`
  .

## 为什么

你也可以在webpack中使用[babel-loader](https://github.com/babel/babel-loader)，引入`minify`[作为一个预设](https://github.com/babel/minify#babel-preset)并且应该运行的更快 - 因为`babel-minify`将运行在更小的文件。但是，这个插件为什么还存在呢？

* webpack loader 对单个文件进行操作，并且 minify preset 作为一个 webpack loader将会把每个文件视为在浏览器全局范围内直接执行（默认情况下），并且不会优化顶级作用域内的某些内容。要在文件的顶级作用域内进行优化，请在 minifyOptions 中设置`mangle: { topLevel: true }`。
* 当你排除 `node_modules`不通过 babel-loader 运行时，babel-minify 优化不会应用于被排除的文件，因为它们不会通过 minifier。
* 当您使用带有 webpack 的 babel-loader 时，由 webpack 为模块系统生成的代码不会通过 loader，并且不会通过 babel-minify 进行优化。
* 一个 webpack 插件可以在整个 chunk/bundle 输出上运行，并且可以优化整个bundle，你可以看到一些细微的输出差异。但是，由于文件大小通常非常大，所以会慢很多。所以这里有[一个想法](https://github.com/webpack-contrib/babel-minify-webpack-plugin/issues/8)- 我们可以将一些优化作为 loader 的一部分，并在插件中进行一些优化。



