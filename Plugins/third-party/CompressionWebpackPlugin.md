# CompressionWebpackPlugin

[![](https://img.shields.io/badge/Github-查看更多-brightgreen.svg)](https://github.com/webpack-contrib/compression-webpack-plugin)

提供带 Content-Encoding 编码的压缩版的资源

## 安装

```bash
npm i -D compression-webpack-plugin
```

## 用法

```js
const CompressionPlugin = require("compression-webpack-plugin")

module.exports = {
  plugins: [
    new CompressionPlugin(...options)
  ]
}
```

## Options

| 名称 | 类型 | 默认值 | 描述 |
| :---: | :---: | :---: | :--- |
| `test` | `{RegExp}` | `.` | 处理所有匹配此正则表达式的资源 |
| `asset` | `{String}` | `[path].gz[query]` | 目标资源名称。`[file]` 会被替换成原资源。`[path]` 会被替换成原资源路径，`[query]` 替换成原查询字符串 |
| `filename` | `{Function}` | `false` | 一个 `{Function} (asset) => asset` 函数，接收原资源名（通过 `asset` 选项）返回新资源名 |
| `algorithm` | `{String\|Function}` | `gzip` | 可以是 `(buffer, cb) => cb(buffer)` 或者是使用 `zlib` 里面的算法的 `{String}` |
| `threshold` | `{Number}` | `0` | 只处理比这个值大的资源。按字节计算 |
| `minRatio` | `{Number}` | `0.8` | 只有压缩率比这个值小的资源才会被处理 |
| `deleteOriginalAssets` | `{Boolean}` | `false` | 是否删除原资源 |

### 示例

```js
const CompressionPlugin = require("compression-webpack-plugin")

const options = {
  test: /\.js/,
  asset: '[path].gz[query]',
  filename(asset) {
    asset = 'rename',
    return asset
  },
  algorithm: 'gzip',
   threshold: 0,
   minRatio: 1,
   deleteOriginalAssets: true
}

module.exports = {
  plugins: [
    new CompressionPlugin(...options)
  ]
}
```



