# [ ](#)HtmlWebpackPlugin

[![](https://img.shields.io/badge/Github-查看更多-brightgreen.svg)](https://github.com/jantimon/html-webpack-plugin)

[![npm version](https://badge.fury.io/js/html-webpack-plugin.svg)](http://badge.fury.io/js/html-webpack-plugin) [![Dependency Status](https://david-dm.org/jantimon/html-webpack-plugin.svg)](https://david-dm.org/jantimon/html-webpack-plugin) [![Build status](https://travis-ci.org/jantimon/html-webpack-plugin.svg)](https://travis-ci.org/jantimon/html-webpack-plugin) [![Windows build status](https://ci.appveyor.com/api/projects/status/github/jantimon/html-webpack-plugin?svg=true&branch=master)](https://ci.appveyor.com/project/jantimon/html-webpack-plugin) [![js-semistandard-style](https://img.shields.io/badge/code style-semistandard-brightgreen.svg?style=flat-square)](https://github.com/Flet/semistandard) [![bitHound Dependencies](https://www.bithound.io/github/jantimon/html-webpack-plugin/badges/dependencies.svg)](https://www.bithound.io/github/jantimon/html-webpack-plugin/master/dependencies/npm) [![license](https://img.shields.io/github/license/mashape/apistatus.svg?maxAge=2592000)]()

[![NPM](https://nodei.co/npm/html-webpack-plugin.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/html-webpack-plugin/)

[`HtmlWebpackPlugin`](#)简化了HTML文件的创建，以便为您的webpack包提供服务。 这对于在文件名中包含每次会随着变异会发生变化的哈希的webpack bundle尤其有用。 您可以让插件为您生成一个HTML文件，使用[lodash模板](#)提供您自己的模板，或使用您自己的[loader](#)。

维护人员: Jan Nicklas [@jantimon](https://twitter.com/jantimon) and Thomas Sileghem [@mastilver](https://twitter.com/mastilver)

## 安装

使用npm安装：

```shell
$ npm install html-webpack-plugin --save-dev
```

## 第三方插件:

html-webpack-plugin 提供了[钩子](#事件)来扩展它以满足您的需要。  
已经有一些非常强大的插件可以与之零配置集成:

| 名称 | 描述 |
| :--- | :--- |
| [webpack-subresource-integrity](https://www.npmjs.com/package/webpack-subresource-integrity) | 用于加强资源安全 |
| [appcache-webpack-plugin](https://github.com/lettertwo/appcache-webpack-plugin) | 用于iOS和Android离线使用 |
| [favicons-webpack-plugin](https://github.com/jantimon/favicons-webpack-plugin) | 它为iOS、Android和桌面浏览器生成了favicons和icons。 |
| [html-webpack-harddisk-plugin](https://github.com/jantimon/html-webpack-harddisk-plugin) | 可以用于将html文件写入磁盘，当使用webpack-dev-server / HMR 时 |
| [html-webpack-inline-source-plugin](https://github.com/DustinJackson/html-webpack-inline-source-plugin) | 用于在生成的HTML文件中内联您的资源 |
| [html-webpack-inline-svg-plugin](https://github.com/thegc/html-webpack-inline-svg-plugin) | 在生成的HTML文件中使用内嵌的SVGs |
| [html-webpack-exclude-assets-plugin](https://github.com/jamesjieye/html-webpack-exclude-assets-plugin) | 使用正则表达式排除资源 |
| [html-webpack-include-assets-plugin](https://github.com/jharris4/html-webpack-include-assets-plugin) | 包含js或css文件路径的列表\(如copy-webpack-plugin复制的文件\)。 |
| [script-ext-html-webpack-plugin](https://github.com/numical/script-ext-html-webpack-plugin) | 添加 `async`, `defer` 或者 `module` 属性到`<script>` 元素，或者甚至是内联他们 |
| [style-ext-html-webpack-plugin](https://github.com/numical/style-ext-html-webpack-plugin) | 将`<link>的外部样式表转换到<style>元素包含的内部`CSS |
| [resource-hints-webpack-plugin](https://github.com/jantimon/resource-hints-webpack-plugin) | `使用<link rel='preload'>` 和 `<link rel='prefetch'>添加资源提示以获得更快的初始页面加载` |
| [preload-webpack-plugin](https://github.com/GoogleChrome/preload-webpack-plugin) | 使用`<link rel='preload'>`协助懒加载来自动连接异步\(和其他类型\)的JavaScript块，。 |
| [link-media-html-webpack-plugin](https://github.com/yaycmyk/link-media-html-webpack-plugin) | 允许注入样式表`<link />`标签来自动地设置它们的媒体属性;可以提供特定的桌面/移动/打印等样式表，浏览器将有条件地下载这些样式表 |
| [inline-chunk-manifest-html-webpack-plugin](https://github.com/jouni-kantola/inline-chunk-manifest-html-webpack-plugin) | 用于内联webpack的chunk清单。默认提取清单并内联进`<head>` |

## 基础用法

该插件将为您生成一个HTML5文件，其中包括使用`script`标签的body中的所有webpack包。 只需添加插件到您的webpack配置如下：

```js
var HtmlWebpackPlugin = require('html-webpack-plugin');
var webpackConfig = {
  entry: 'index.js',
  output: {
    path: __dirname + '/dist',
    filename: 'index_bundle.js'
  },
  plugins: [new HtmlWebpackPlugin()]
};
```

这将会产生一个包含以下内容的文件`dist/index.html`：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Webpack App</title>
  </head>
  <body>
    <script src="index_bundle.js"></script>
  </body>
</html>
```

如果您有多多个webpack入口点，他们都会在生成的HTML文件中的`script`标签内。

如果你有任何CSS assets 在webpack的输出中（例如，利用[ExtractTextPlugin](/Plugins/third-party/ExtractTextWebpackPlugin.md)提取CSS），那么这些将被包含在HTML head中的`<link>`标签内。

## 配置

您可以将配置选项的哈希传递给`HtmlWebpackPlugin。`允许的值如下:

* `title`：用于生成的HTML文档的标题。
* `filename`：将HTML写入的文件。默认为`index.html`。您也可以在这里指定一个子目录\(例如:`assets/admin.html`\)。
* `template`： webpack加载模板的路径。请查看 [这个文档](https://github.com/jantimon/html-webpack-plugin/blob/master/docs/template-option.md) 获取更多信息。
* `inject`：`true | 'head' | 'body' | false` 将所有的资源注入到给定的`template`或`templateContent`中——当传递`true`或`'body'`时，所有的javascript资源将被放置在body元素的底部。`'head'`表示将把脚本放在head元素中。
* `favicon`：将给定的favicon路径添加到输出html。
* `minify`：`{...} | false` 传递将[html-minifier](https://github.com/kangax/html-minifier#options-quick-reference)的选项作为对象，以缩小输出。
* `hash`：`true | false` 如果为 `true` 然后向所有包含的脚本和CSS文件附加一个惟一的webpack编译哈希。这对于缓存的破坏非常有用。
* `cache`： `true | false` 如果为 `true` \(默认\)，只有在更改时才尝试发射该文件。 
* `showErrors`: `true | false` 如果为 `true` \(默认\)， 错误细节将被写入HTML页面。
* `chunks`：允许您只添加一些块\(例如，只有unit-test 块\)
* `chunksSortMode`：允许在将块包含到html之前控制块的排序。允许的值：`'none'` \|`'auto'` \|`'dependency'` \|`'manual'`\| `{function}` - 默认值：`'auto'`
* `excludeChunks`：允许你跳过一些块 \(例如. 不添加 unit-test 块\)
* `xhtml`：`true | false` 如果为 `true`，将`link`标记渲染为自闭、XHTML兼容。默认是`false`

下面是一个webpack配置示例，说明如何使用这些选项:

```javascript
{
  entry: 'index.js',
  output: {
    path: __dirname + '/dist',
    filename: 'index_bundle.js'
  },
  plugins: [
    new HtmlWebpackPlugin({
      title: 'My App',
      filename: 'assets/admin.html'
    })
  ]
}
```

## 常见问题

* [为什么我的HTML被缩小了?](https://github.com/jantimon/html-webpack-plugin/blob/master/docs/template-option.md)
* [为什么 我的`<% ... %>` 模板 不工作?](https://github.com/jantimon/html-webpack-plugin/blob/master/docs/template-option.md)
* [我如何把车把](https://github.com/jantimon/html-webpack-plugin/blob/master/docs/template-option.md)[handlebars/pug/ejs](https://github.com/jantimon/html-webpack-plugin/blob/master/docs/template-option.md)[作为模板引擎？](https://github.com/jantimon/html-webpack-plugin/blob/master/docs/template-option.md)

## 生成多个HTML文件

要生成多个HTML文件，请在您的插件数组中声明一个以上的插件:

```js
{
  entry: 'index.js',
  output: {
    path: __dirname + '/dist',
    filename: 'index_bundle.js'
  },
  plugins: [
    new HtmlWebpackPlugin(), // 生成默认的 index.html
    new HtmlWebpackPlugin({  // 生成一个 test.html
      filename: 'test.html',
      template: 'src/assets/test.html'
    })
  ]
}
```

## 编写自己的模板

如果默认生成的HTML无法满足您的需求，您可以提供自己的模板。最简单的方法是使用`template`选项并传递一个自定义的HTML文件。html-webpack-plugin将自动将所有必要的CSS、JS、manifest和favicon文件注入到标记中。

```js
plugins: [
  new HtmlWebpackPlugin({
    title: 'Custom template',
    template: 'my-index.html', // 加载一个自定义模板 (在默认情况下，lodash会查看FAQ)
  })
]
```

`my-index.html`:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta http-equiv="Content-type" content="text/html; charset=utf-8"/>
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>
  </body>
</html>
```

如果您已经有了一个模板loader，那么您可以使用它来解析模板。请注意，如果您指定了html-loader和使用`.html`作为模板也会发生这种情况。

```js
module: {
   loaders: [
    { test: /\.hbs$/, loader: "handlebars" }
  ]
},
plugins: [
  new HtmlWebpackPlugin({
    title: 'Custom template using Handlebars',
    template: 'my-index.hbs'
  })
]
```

您可以使用开箱即用的lodash语法。  
如果`inject`特性不符合您的需求，并且您想要完全控制资源放置，那么您可以使用 [html-webpack-template 项目 ](https://github.com/jaketrent/html-webpack-template)的 [默认模板 ](https://github.com/jaketrent/html-webpack-template/blob/86f285d5c790a6c15263f5cc50fd666d51f974fd/index.html)作为您自己编写的起点。

下面的变量可以在模板中找到:

* `htmlWebpackPlugin`: 特定于此插件的数据

  * `htmlWebpackPlugin.files` ：一个对webpack的[stats](https://github.com/webpack/docs/wiki/node.js-api#stats)对象的`assetsByChunkName`属性的一个按摩（massaged）的表示。它包含一个从入口点名到包文件名的映射，例如:

    ```json
    "htmlWebpackPlugin": {
      "files": {
        "css": [ "main.css" ],
        "js": [ "assets/head_bundle.js", "assets/main_bundle.js"],
        "chunks": {
          "head": {
            "entry": "assets/head_bundle.js",
            "css": [ "main.css" ]
          },
          "main": {
            "entry": "assets/main_bundle.js",
            "css": []
          },
        }
      }
    }
    ```

    如果您已经在webpack配置中设置了公共路径（publicPath），那么这将在这个资源哈希中得到正确的反映。

  * `htmlWebpackPlugin.options`：将传递给插件的选项哈希。除了这个插件实际使用的选项之外，您还可以使用这个哈希将任意数据传递给模板。

* `webpack`：webpack [stats](https://github.com/webpack/docs/wiki/node.js-api#stats)对象。请注意，这是stats对象，就像它在HTML模板被发射时所做的那样，因为在webpack运行完成之后，它可能没有完整的统计数据集。

* `webpackConfig`: 用于此编译的webpack配置。这可以被使用,例如,获取`publicPath`\(`webpackConfig.output.publicPath`\)。

## 过滤chunk

要只包含特定的chun，你可以限制chun被使用:

```js
plugins: [
  new HtmlWebpackPlugin({
    chunks: ['app']
  })
]
```

还可以通过设置`excludeChunks`选项来排除某些chunk:

```javascript
plugins: [
  new HtmlWebpackPlugin({
    excludeChunks: ['dev-helper']
  })
]
```

## 事件

为了允许其他[插件](https://github.com/webpack/docs/wiki/plugins)改变HTML，这个插件执行以下事件:

异步:

* `html-webpack-plugin-before-html-generation`
* `html-webpack-plugin-before-html-processing`
* `html-webpack-plugin-alter-asset-tags`
* `html-webpack-plugin-after-html-processing`
* `html-webpack-plugin-after-emit`

同步:

* `html-webpack-plugin-alter-chunks`

示例实现: [html-webpack-harddisk-plugin](https://github.com/jantimon/html-webpack-harddisk-plugin)

用法：

```javascript
// MyPlugin.js

function MyPlugin(options) {
  // 使用options配置你的插件...
}

MyPlugin.prototype.apply = function(compiler) {
  // ...
  compiler.plugin('compilation', function(compilation) {
    console.log('The compiler is starting a new compilation...');

    compilation.plugin('html-webpack-plugin-before-html-processing', function(htmlPluginData, callback) {
      htmlPluginData.html += 'The magic footer';
      callback(null, htmlPluginData);
    });
  });

};

module.exports = MyPlugin;
```

然后在 `webpack.config.js`使用

```javascript
plugins: [
  new MyPlugin({options: ''})
]
```

请注意，必须传递htmlPluginData给回调，以便将其传递到其他任何使用相同`html-webpack-plugin-before-html-processing`事件的插件中。

## 模板选项

### History

2015年9月版本2.x改变了模板的处理方式。它没有强制所有用户使用 [blueimp](https://github.com/blueimp/JavaScript-Templates) 模板引擎，而是允许使用任何webpack loader:

* [jade/pug](https://github.com/pugjs/pug-loader)
* [ejs](https://github.com/okonet/ejs-loader)
* [underscore](https://github.com/emaphp/underscore-template-loader)
* [handlebars](https://github.com/pcardune/handlebars-loader)
* [html-loader](https://github.com/webpack/html-loader)
* ...

在外壳之下，它使用了一个webpack子编译，它继承了主配置中的所有loader。

有三种方法来设置loader:

### 不设置任何loader

默认情况下\(如果您不以任何方式指定任何loader\)，一个备用的[ lodash loader](https://github.com/ampedandwired/html-webpack-plugin/blob/master/lib/loader.js)就会启动。

```js
{
  plugins: [
    new HtmlWebpackPlugin({
      template: 'src/index.html'
    })
  ]
}
```

请注意,使用。html作为模板扩展可能会意外地触发另一个加载器。

> **\[info\]** 注：
>
> 使用`.html`作为模板扩展可能会意外地触发另一个loader。

### 为模板直接设置loader

```js
new HtmlWebpackPlugin({
  // For details on `!!` see https://webpack.github.io/docs/loaders.html#loader-order
  template: '!!handlebars!src/index.hbs'
})
```

### 使用`module.loaders` 语法设置loader

```js
{
  module: {
    loaders: [
      {
        test: /\.hbs$/,
        loader: 'handlebars-loader'
      },
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: 'src/index.hbs'
    })
  ]
}
```

但是，这也意味着在下面的示例中，webpack将为您的模板使用[html loader](https://webpack.js.org/loaders/html-loader/)。这将**导致html缩小**，它也会**禁用ejs备用的loader**。

```js
{
  module: {
    loaders: [
      {
        test: /\.html$/,
        loader: 'html-loader'
      }],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: 'src/index.html'
    })
  ]
}
```



