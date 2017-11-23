# ZopfliWebpackPlugin

[![](https://img.shields.io/badge/Github-查看更多-brightgreen.svg)](https://github.com/webpack-contrib/zopfli-webpack-plugin)

Node-Zopfli Webpack插件。使用node-zopfli准备压缩版本的资源。

## 安装

```js
npm i -D zopfli-webpack-plugin
```

## 用法

```js
var ZopfliPlugin = require("zopfli-webpack-plugin");
module.exports = {
  plugins: [
    new ZopfliPlugin({
      asset: "[path].gz[query]",
      algorithm: "zopfli",
      test: /\.(js|html)$/,
      threshold: 10240,
      minRatio: 0.8
    })
  ]
}
```

## 参数

* `asset`：目标资源名称。`[file]`被替换为原始资源。`[path]`被替换为原始资源的路径和`[query]` 。默认为`"[path].gz[query]"`。
* `filename`：一个`function(asset)`，它接收资源名称\(在处理资源选项后\)并返回新的资源名称。默认值为`false`。
* `algorithm`：算法。可以是一个`function(buf, callback)`或字符串。对于一个字符串，算法（`algorithm`）是从`zopfli`提取的。
* `test`：所有匹配该RegExp的资源都被处理。默认为所有资源。
* `threshold`：阀值。仅处理大于此大小的资源。单位：字节。默认值为`0`。
* `minRatio`：只有此压缩比的资源才会得到处理。默认为`0.8`。
* `deleteOriginalAssets`：是否删除原始资源。默认值为`false`。

## 可选参数

* verbose: 默认值: false,

* verbose\_more: 默认值:: false,

* numiterations: 默认值:: 15,
* blocksplitting: 默认值:: true,
* blocksplittinglast: 默认值:: false,
* blocksplittingmax: 默认值:: 15



