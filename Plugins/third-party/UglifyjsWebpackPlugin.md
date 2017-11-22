# UglifyjsWebpackPlugin

这个插件使用 [UglifyJS v3](https://github.com/mishoo/UglifyJS2/tree/harmony)[\(`uglify-es`\)](https://npmjs.com/package/uglify-es) 来压缩你的JavaScript。

> **\[info\]**
>
> `webpack =< v3.0.0`目前在`webpack.optimize.UglifyJsPlugin`下包含这个插件的[`v0.4.6`](https://github.com/webpack-contrib/uglifyjs-webpack-plugin/tree/version-0.4) 版本的别名。对于最新版本的使用 \(`v1.0.0`\)，请跟随下面的步骤， `v1.0.0`预计将会在`webpack v4.0.0`中作为`webpack.optimize.UglifyJsPlugin`存在。

## 安装

```js
npm i -D uglifyjs-webpack-plugin
```

## 用法

**webpack.config.js**

```js
const UglifyJSPlugin = require('uglifyjs-webpack-plugin')

module.exports = {
  plugins: [
    new UglifyJSPlugin(options)
  ]
}
```

## 选项



