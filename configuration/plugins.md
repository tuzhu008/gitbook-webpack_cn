# Plugins（插件）

`plugins`选项用于以多种方式定制webpack构建过程。webpack在`webpack.[plugin-name]`下提供了各种各样的内置插件。请参阅这一页的插件和文档列表，但请注意，社区中有更多的插件和文档。

> **\[info\] 注：**
>
> 本页只讨论使用插件，但如果你有兴趣写自己的插件，请访问[如何写插件](https://doc.webpack-china.org/development/how-to-write-a-plugin/)。
>
> 如果想要查看更多插件的使用文档，请查阅[这个文档。](/Plugins/README.md)

```js
module.exports = {
    plugins: [
        ...
    ]
}
```

## `plugins`

类型：array

webpack 插件列表。例如，当多个 bundle 共享一些相同的依赖，`CommonsChunkPlugin`有助于提取这些依赖到共享的 bundle 中，来避免重复打包。可以像这样添加：

```js
plugins: [
  new webpack.optimize.CommonsChunkPlugin({
    //配置参数
  })
]
```

使用多个插件，可能看起来就像这样：

```js
var webpack = require('webpack')
// 导入非 webpack 默认自带插件
var ExtractTextPlugin = require('extract-text-webpack-plugin');
var DashboardPlugin = require('webpack-dashboard/plugin');

// 在配置中添加插件
plugins: [
  // 构建优化插件
  new webpack.optimize.CommonsChunkPlugin({
    name: 'vendor',
    filename: 'vendor-[hash].min.js',
  }),
  new webpack.optimize.UglifyJsPlugin({
    compress: {
      warnings: false,
      drop_console: false,
    }
  }),
  new ExtractTextPlugin({
    filename: 'build.min.css',
    allChunks: true,
  }),
  new webpack.IgnorePlugin(/^\.\/locale$/, /moment$/),
  // 编译时(compile time)插件
  new webpack.DefinePlugin({
    'process.env.NODE_ENV': '"production"',
  }),
  // webpack-dev-server 强化插件
  new DashboardPlugin(),
  new webpack.HotModuleReplacementPlugin(),
]
```



