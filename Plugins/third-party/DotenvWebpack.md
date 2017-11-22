# DotenvWebpack

[![](https://img.shields.io/badge/Github-查看更多-brightgreen.svg)](https://github.com/mrsteele/dotenv-webpack)

[DefinePlugin](/Plugins/internal/DefinePlugin.md)一个安全的webpack插件，它支持[dotenv](https://github.com/motdotla/dotenv)和其他环境变量，**只暴露被选择和使用的内容**。

## 安装

使用npm安装

`npm install dotenv-webpack --save`

## 描述

`dotenv-webpack` 封装了[`dotenv`](https://github.com/motdotla/dotenv) 和 [`Webpack.DefinePlugin`](/Plugins/internal/DefinePlugin.md)。因此，它为任何`process.env`的实例都替换了结果bundle中的文本。

你的`.env`文件可以包含敏感信息。因此，`dotenv-webpack`只会暴露在您的代码中**被明确引用的**环境变量到您的最终bundle中。

## 用法

这个插件可以安装在很少到不需要的配置上。安装之后，您可以在代码中使用`process.env`访问变量，就像你使用`dotenv`一样。

示例显示了一个标准用例。

###### 创建一个 `.env` 文件

```
// .env
DB_HOST=127.0.0.1
DB_PASS=foobar
S3_API=mysecretkey
```

###### 把它添加到你的Webpack配置文件

```javascript
// webpack.config.js
const Dotenv = require('dotenv-webpack');

module.exports = {
  ...
  plugins: [
    new Dotenv()
  ]
  ...
};
```

###### 在你的代码中使用

```javascript
// file1.js
console.log(process.env.DB_HOST);
// '127.0.0.1'
```

###### 结果bundle

```javascript
// bundle.js
console.log('127.0.0.1');
```

> **\[info\] 注：**
>
> `.env`中的`DB_PASS`和`S3_API`在我们的bindle中并不存在，因为它们在代码中从未被引用\(作为`process.env.[VAR_NAME]`\)。

## 为什么是安全的?

允许您准确地定义您从何处加载环境变量，并且只打包在项目代码中被明确引用的变量，您可以确定只需要包含哪些内容，并且不会意外地泄漏任何敏感信息。

###### 推荐

添加 `.env` 到你的 `.gitignore` 文件，以忽略它。

## 属性

使用以下属性配置您的实例。

* **path** \(`'./.env'`\) - 环境变量文件所在路径。
* **safe** \(`false`\) -  如果为false，则忽略安全模式；如果为true，则加载`'./.env.example'`，如果是一个字符串，则加载这个字符串路径代表的文件作为样本。
* **systemvars** \(`false`\) - 如果您更愿意加载所有系统变量，则设置为true\(用于CI目的\)。
* **silent** \(`false`\) - If true, all warnings will be surpressed.如果为true，所有的警告都会被抑制。

下面的例子展示了如何设置所有的参数。

```javascript
module.exports = {
  ...
  plugins: [
    new Dotenv({
      path: './some.other.env', // 现在加载这个，而不是在'.env'中
      safe: true, // 加载 '.env.example' 来验证 '.env' 变量被全部设置了。也可以是另一个文件的路径字符串。
      systemvars: true, // 加载所有预设的'process.env'变量，它将胜过任何本地的dotenv规范。
      silent: true // 隐藏任何错误
    })
  ]
  ...
};
```

## LICENSE

MIT

