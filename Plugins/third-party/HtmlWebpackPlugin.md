# [ ](#)HtmlWebpackPlugin

[![](https://img.shields.io/badge/Github-查看更多-brightgreen.svg)](https://github.com/jantimon/html-webpack-plugin)

[`HtmlWebpackPlugin`](https://github.com/ampedandwired/html-webpack-plugin)简化了HTML文件的创建，以便为您的webpack包提供服务。 这对于在文件名中包含每次会随着变异会发生变化的哈希的webpack bundle尤其有用。 您可以让插件为您生成一个HTML文件，使用[lodash模板](https://lodash.com/docs#template)提供您自己的模板，或使用您自己的[loader](https://doc.webpack-china.org/loaders)。

## 安装

```bash
npm install --save-dev html-webpack-plugin
```

## 用法

该插件将为您生成一个HTML5文件，其中包括使用`script`标签的body中的所有webpack包。 只需添加插件到您的webpack配置如下：

```js
var HtmlWebpackPlugin = require('html-webpack-plugin');

var webpackConfig = {
  entry: 'index.js',
  output: {
    path: 'dist',
    filename: 'index_bundle.js'
  },
  plugins: [
    new HtmlWebpackPlugin()
  ]
};
```

这将会产生一个包含以下内容的文件`dist/index.html`：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>webpack App</title>
  </head>
  <body>
    <script src="index_bundle.js"></script>
  </body>
</html>
```

如果您有多多个webpack入口点，他们都会在生成的HTML文件中的`script`标签内。

如果你有任何CSS assets 在webpack的输出中（例如，利用[ExtractTextPlugin](https://doc.webpack-china.org/plugins/extract-text-webpack-plugin)提取CSS），那么这些将被包含在HTML head中的`<link>`标签内。



