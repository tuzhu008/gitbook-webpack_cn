# raw-loader

raw-loader让您以字符串的形式导入文件。

[![](https://img.shields.io/badge/Github-查看更多-brightgreen.svg)](https://github.com/webpack-contrib/raw-loader)

## 安装

```bash
npm install --save-dev raw-loader
```

## 用法

通过 webpack 配置、命令行或者内联使用 loader。

### 通过webpack 配置 \(推荐\)

**webpack.config.js**

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.txt$/,
        use: 'raw-loader'
      }
    ]
  }
}
```

**在应用程序中：**

```js
import txt from './file.txt';
```

### CLI

```bash
webpack --module-bind 'txt=raw-loader'
```

**在应用程序中：**

```js
import txt from 'file.txt';
```

### Inline

**在应用程序中：**

```js
import txt from 'raw-loader!./file.txt';
```



