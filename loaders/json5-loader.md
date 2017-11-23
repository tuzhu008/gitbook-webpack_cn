# json5-loader

[![](https://img.shields.io/badge/Github-查看更多-brightgreen.svg)](https://github.com/webpack-contrib/json5-loader)

将[`json5`](http://json5.org/)文件解析为JavaScript对象。

## 安装

```bash
npm install --save-dev json5-loader
```

## 用法

你可以通过以下用法使用这个 loader

* 在 webpack 配置里的`module.loaders`对象中配置`json5-loader`；
* 直接在 require 语句中使用`json5-loader!`前缀。

假设我们有下面这个`json5`文件

```js
// appData.json5
{
  env: 'production',
  passwordStregth: 'strong'
}
```

### webpack配置

```js
// webpack.config.js
module.exports = {
  entry: './index.js',
  output: { /* ... */ },
  module: {
    loaders: [
      {
        // 使所有以 .json5结尾的文件使用 `json5-loader`
        test: /\.json5$/,
        loader: 'json5-loader'
      }
    ]
  }
}
```

```js
// index.js
var appConfig = require('./appData.json5')
// or, in ES6
// import appConfig from './appData.json5'

console.log(appConfig.env) // 'production'
```

### inline

```js
var appConfig = require("json5-loader!./appData.json5")
// returns the content as json parsed object

console.log(appConfig.env) // 'production'
```



