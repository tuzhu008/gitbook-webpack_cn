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
  // Absolute path to your webpack root folder (paths appended to this)webpack根目录的绝对路径（）
  // Default: root of your package
  root: __dirname,

  // Write logs to console.
  verbose: true,

  // Use boolean "true" to test/emulate delete. (will not remove files).
  // Default: false - remove files
  dry: false,           

  // If true, remove files on recompile. 
  // Default: false
  watch: false,

  // Instead of removing whole path recursively,
  // remove all path's content with exclusion of provided immediate children.
  // Good for not removing shared files from build directories.
  exclude: [ 'files', 'to', 'ignore' ],

  // allow the plugin to clean folders outside of the webpack root.
  // Default: false - don't allow clean folder outside of the webpack root
  allowExternal: false
}
```



