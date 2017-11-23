# Clean for WebPack

在构建之前，可以删除/清理您的构建文件夹\(s\)

![MIT License](https://camo.githubusercontent.com/d59450139b6d354f15a2252a47b457bb2cc43828/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f6c2f7365727665726c6573732e737667)  
[![](https://travis-ci.org/johnagan/clean-webpack-plugin.svg)](https://travis-ci.org/johnagan/clean-webpack-plugin)  
[![](https://coveralls.io/repos/johnagan/clean-webpack-plugin/badge.svg)](https://coveralls.io/github/johnagan/clean-webpack-plugin)

## 安装

```
npm i clean-webpack-plugin --save-dev
```

## 用法

```js
const CleanWebpackPlugin = require('clean-webpack-plugin')

// webpack config
{
  plugins: [
    new CleanWebpackPlugin(paths [, {options}])
  ]
}
```

## 示例

这是 [WebPack 的插件文档](https://webpack.js.org/concepts/plugins/) 的修改版本，它包含了Clean插件。

```js
const CleanWebpackPlugin = require('clean-webpack-plugin'); //通过npm安装的
const HtmlWebpackPlugin = require('html-webpack-plugin'); //通过npm安装的
const webpack = require('webpack'); // 用来访问内置插件
const path = require('path');

// 应该被清理的路径(s)
let pathsToClean = [
  'dist',
  'build'
]

// 使用的清洁选项
let cleanOptions = {
  root:     '/full/webpack/root/path',
  exclude:  ['shared.js'],
  verbose:  true,
  dry:      false
}

// 简单的webpack配置
const webpackConfig = {
  entry: './path/to/my/entry/file.js',
  output: {
    filename: 'my-first-webpack.bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        loader: 'babel-loader'
      }
    ]
  },
  plugins: [
    new CleanWebpackPlugin(pathsToClean, cleanOptions),
    new webpack.optimize.UglifyJsPlugin(),
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
}
```

### 路径 \(必须\)

被清理的路径数组

```js
[
  'dist',         // 移除 'dist' 文件夹
  'build/*.*',    // 'build' 文件夹下的所有文件
  'web/*.js'      // 移除 'web' 文件夹下的javascrit文件
]
```

### 选项及其默认值 \(可选的\)

```js
{
  // webpack根目录的绝对路径（删除路径附加到这里）
  // 默认值: 包的根目录
  root: __dirname,

  // 将日志写到控制台
  verbose: true,

  // 设置为 "true" 来 测试/模拟 删除. (不会删除文件).
  // 默认: false - 删除文件
  dry: false,           

  // 如果为 true, 在编译时删除文件。
  // 默认: false
  watch: false,

  // 不再是递归地删除整个路径，
  // 删除所有路径的内容，除了提供的需要排除的。
  // 不删除构建目录中的共享文件是很的实践。
  exclude: [ 'files', 'to', 'ignore' ],

  // 允许插件在webpack根之外清除文件夹。  
  // 默认值: false - 不允许
  allowExternal: false
}
```



