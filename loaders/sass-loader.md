[![npm][npm]][npm-url]
[![node][node]][node-url]
[![npm-stats][npm-stats]][npm-url]
[![deps][deps]][deps-url]
[![travis][travis]][travis-url]
[![appveyor][appveyor]][appveyor-url]
[![coverage][cover]][cover-url]
[![chat][chat]][chat-url]

<div align="center">
  <img height="100"
    src="https://worldvectorlogo.com/logos/sass-1.svg">
  <a href="https://github.com/webpack/webpack">
    <img width="200" height="200"
      src="https://webpack.js.org/assets/icon-square-big.svg">
  </a>
</div>
<h1>SASS Loader</h1>

[![](https://img.shields.io/badge/Github-%E6%9F%A5%E7%9C%8B%E6%9B%B4%E5%A4%9A-brightgreen.svg)](https://github.com/webpack-contrib/sass-loader)

加载 SASS/SCSS 文件，并将其编译为CSS。

使用 [css-loader](//loaders/css-loader.md) 或者  [raw-loader](/loaders/raw-loader.md) 将其转换为 JS模块，使用 [ExtractTextPlugin](/Plugins/third-party/ExtractTextWebpackPlugin.md) 将其提取到一个单独的文件。 
寻找 webpack 1 loader? 查看 [archive/webpack-1 branch](https://github.com/webpack-contrib/sass-loader/tree/archive/webpack-1).

<h2>安装</h2>

```bash
npm install sass-loader node-sass webpack --save-dev
```

sass-loader 需要 [node-sass](https://github.com/sass/node-sass) 和 [webpack](https://github.com/webpack)
作为 [`同级依赖(peerDependency)`](https://docs.npmjs.com/files/package.json#peerdependencies)。这样您就能够准确地控制这些版本。

<h2>示例</h2>

将 sass-loader 与 [css-loader](//loaders/css-loader.md) 和 [style-loader](/loaders/style-loader.md) 链接起来，让所有样式立即被应用到DOM 。

```js
// webpack.config.js
module.exports = {
	...
    module: {
        rules: [{
            test: /\.scss$/,
            use: [{ // 从下往上执行
                loader: "style-loader" // 从JS字符串中创建style节点
            }, {
                loader: "css-loader" // 转换 CSS 到 CommonJS
            }, {
                loader: "sass-loader" // 编译 Sass 到 CSS
            }]
        }]
    }
};
```
你也可以通过指定一个`options`属性，直接传递选项到[node-sass](https://github.com/andrew/node-sass) ，像这样

```js
// webpack.config.js
module.exports = {
	...
    module: {
        rules: [{
            test: /\.scss$/,
            use: [{
                loader: "style-loader"
            }, {
                loader: "css-loader"
            }, {
                loader: "sass-loader",
                options: {
                    includePaths: ["absolute/path/a", "absolute/path/b"]
                }
            }]
        }]
    }
};
```

参见 [node-sass](https://github.com/andrew/node-sass) 获取更多可用的 Sass 选项。

### 在生产中

通常，生产环境下比较推荐的做法是，使用 [ExtractTextPlugin](/Plugins/third-party/ExtractTextWebpackPlugin.md) 将样式表抽离成专门的单独文件。这样，样式表将不再依赖于 JavaScript：

```js
const ExtractTextPlugin = require("extract-text-webpack-plugin");

const extractSass = new ExtractTextPlugin({
    filename: "[name].[contenthash].css",
    disable: process.env.NODE_ENV === "development"
});

module.exports = {
	...
    module: {
        rules: [{
            test: /\.scss$/,
            use: extractSass.extract({
                use: [{
                    loader: "css-loader"
                }, {
                    loader: "sass-loader"
                }],
                // 在开发环境使用 style-loader 
                fallback: "style-loader"
            })
        }]
    },
    plugins: [
        extractSass
    ]
};
```

<h2>用法</h2>

### 导入

webpack 提供一种[解析文件的高级机制](https://webpack.js.org/concepts/module-resolution/)。[sass-loader](/loaders/sass-loader.md) 使用 [node-sass](https://github.com/andrew/node-sass) 的 自定义 importer 特性，将所有的查询（query）传递给 webpack 的解析引擎。只要它们前面加上 `~`，告诉 webpack 它不是一个相对路径，这样就可以导入 `node_modules` 目录里面的 Sass 模块：

```css
@import "~bootstrap/dist/css/bootstrap";
```
重要的是，只在前面加上 `~`，因为 `~/` 会解析到主目录(home directory)。webpack 需要区分 `bootstrap` 和 `~bootstrap`，因为 CSS 和 Sass 文件没有用于导入相关文件的特殊语法。`@import "file"` 与 `@import "./file"`这两种写法是相同的


###  `url(...)`的问题

由于 Sass/[libsass](https://github.com/sass/libsass) 并没有提供[url rewriting](https://github.com/sass/libsass/issues/532) 的功能，所以所有的链接资源都是相对输出文件(output)而言。

- 如果生成的 CSS 没有传递给 [css-loader](//loaders/css-loader.md)，它是相对于网站的根目录。
- 如果生成的 CSS 传递给了 [css-loader](//loaders/css-loader.md)，则所有的 url 都相对于入口文件（例如：`main.scss`）。

第二种情况可能会带来一些问题。正常情况下我们期望相对路径的引用是相对于 `.scss` 去解析（如同在 `.css` 文件一样）。幸运的是，有2个方法可以解决这个问题：

- 将 [resolve-url-loader ](/loaders/resolve-url-loader.md)设置于 loader 链中的 sass-loader 之后，就可以重写 url。
- Library 作者一般都会提供变量，用来设置资源路径，如 `bootstrap-sass` 可以通过 `$icon-font-path` 来设置。参见[this working bootstrap example](https://github.com/webpack-contrib/sass-loader/tree/master/test/bootstrapSass).


### 提取样式表

使用 webpack 打包 CSS 有许多优点，在开发环境，可以通过 hashed urls 或 [模块热替换(HMR)](https://webpack.js.org/concepts/hot-module-replacement/) 引用图片和字体资源。而在生产环境，使样式依赖 JS 执行环境并不是一个好的实践。渲染会被推迟，甚至会出现 [FOUC](https://en.wikipedia.org/wiki/Flash_of_unstyled_content)，因此在最终生产环境构建时，最好还是能够将 CSS 放在单独的文件中。

从 bundle 中提取样式表，有2种可用的方法：

- [extract-loader](//loaders/extract-loader.md) （简单，专门针对 css-loader 的输出）
- [extract-text-webpack-plugin](/Plugins/third-party/ExtractTextWebpackPlugin.md) (复杂，但能够处理足够多的场景)


### Source maps

要启用 CSS source map，需要将 `sourceMap` 选项传递给 sass-loader **和** css-loader。此时webpack.config.js 如下：

```javascript
module.exports = {
    ...
    devtool: "source-map", // any "source-map"-like devtool is possible
    module: {
        rules: [{
            test: /\.scss$/,
            use: [{
                loader: "style-loader"
            }, {
                loader: "css-loader", options: {
                    sourceMap: true
                }
            }, {
                loader: "sass-loader", options: {
                    sourceMap: true
                }
            }]
        }]
    }
};
```

如果你要在 Chrome 中编辑原始的 Sass 文件，建议阅读这篇不错的[博客文章](https://medium.com/@toolmantim/getting-started-with-css-sourcemaps-and-in-browser-sass-editing-b4daab987fb0)。具体示例参考 [test/sourceMap](https://github.com/webpack-contrib/sass-loader/tree/master/test)。

### 环境变量

如果你想要将 Sass 代码放在实际的入口文件(entry file)之前，可以设置 `data` 选项。此时 sass-loader 不会覆盖 `data` 选项，只会将它拼接在入口文件的内容之前。当 Sass 变量依赖于环境时，这一点尤其有用。

```javascript
{
    loader: "sass-loader",
    options: {
        data: "$env: " + process.env.NODE_ENV + ";"
    }
}
```

> **[info]** 注：
> 
> 由于代码注入, 会破坏整个入口文件的 source map。通常一个简单的解决方案是，多个 Sass 入口文件。

<h2>维护者</h2>

<table>
    <tr>
      <td align="center">
        <a href="https://github.com/jhnns"><img width="150" height="150" src="https://avatars0.githubusercontent.com/u/781746?v=3"></a><br>
        <a href="https://github.com/jhnns">Johannes Ewald</a>
      </td>
      <td align="center">
        <a href="https://github.com/webpack-contrib"><img width="150" height="150" src="https://avatars1.githubusercontent.com/u/1243901?v=3&s=460"></a><br>
        <a href="https://github.com/webpack-contrib">Jorik Tangelder</a>
      </td>
      <td align="center">
        <a href="https://github.com/akiran"><img width="150" height="150" src="https://avatars1.githubusercontent.com/u/3403295?v=3"></a><br>
        <a href="https://github.com/akiran">Kiran</a>
      </td>
    <tr>
</table>


<h2>License</h2>

[MIT](http://www.opensource.org/licenses/mit-license.php)

[npm]: https://img.shields.io/npm/v/sass-loader.svg
[npm-stats]: https://img.shields.io/npm/dm/sass-loader.svg
[npm-url]: https://npmjs.com/package/sass-loader

[node]: https://img.shields.io/node/v/sass-loader.svg
[node-url]: https://nodejs.org

[deps]: https://david-dm.org/webpack-contrib/sass-loader.svg
[deps-url]: https://david-dm.org/webpack-contrib/sass-loader

[travis]: http://img.shields.io/travis/webpack-contrib/sass-loader.svg
[travis-url]: https://travis-ci.org/webpack-contrib/sass-loader

[appveyor-url]: https://ci.appveyor.com/project/webpack-contrib/sass-loader/branch/master
[appveyor]: https://ci.appveyor.com/api/projects/status/rqpy1vaovh20ttxs/branch/master?svg=true

[cover]: https://codecov.io/gh/webpack-contrib/sass-loader/branch/master/graph/badge.svg
[cover-url]: https://codecov.io/gh/webpack-contrib/sass-loader

[chat]: https://badges.gitter.im/webpack/webpack.svg
[chat-url]: https://gitter.im/webpack/webpack

