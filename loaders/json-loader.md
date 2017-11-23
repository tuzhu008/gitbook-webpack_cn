# JSON Loader

[![](https://img.shields.io/badge/Github-查看更多-brightgreen.svg)](https://github.com/webpack-contrib/json-loader)

## 安装

```js
npm install --save-dev json-loader
```

> **\[info\]** 注：  
> 自 webpack &gt;= v2.0.0起， 默认支持导入 JSON 文件。如果您使用自定义文件扩展名，您可能仍然需要使用此 loader。[**v1.0.0 -&gt; v2.0.0 迁移指南**](https://webpack.js.org/guides/migrating/#json-loader-is-not-required-anymore)获得更多信息

## 用法

### `Inline`

```js
const json = require('json-loader!./file.json');
```

### 配置\(推荐\)

```js
const json = require('./file.json');
```

**webpack.config.js**

```js
module.exports = {
  module: {
    loaders: [
      {
        test: /\.json$/,
        loader: 'json-loader'
      }
    ]
  }
}
```



