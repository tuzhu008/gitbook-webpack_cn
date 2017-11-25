extract-loader
==============
[![](https://img.shields.io/badge/Github-%E6%9F%A5%E7%9C%8B%E6%9B%B4%E5%A4%9A-brightgreen.svg)](https://github.com/peerigon/extract-loader)

**从bundle中提取HTML and CSS**

[![](https://img.shields.io/npm/v/extract-loader.svg)](https://www.npmjs.com/package/extract-loader)
[![](https://img.shields.io/npm/dm/extract-loader.svg)](https://www.npmjs.com/package/extract-loader)
[![Dependency Status](https://david-dm.org/peerigon/extract-loader.svg)](https://david-dm.org/peerigon/extract-loader)
[![Build Status](https://travis-ci.org/peerigon/extract-loader.svg?branch=master)](https://travis-ci.org/peerigon/extract-loader)
[![Coverage Status](https://img.shields.io/coveralls/peerigon/extract-loader.svg)](https://coveralls.io/r/peerigon/extract-loader?branch=master)

extract-loader 动态地对给定的源代码进行计算，并将结果作为字符串返回。它的主要用例是在HTML和CSS中从各自的loader中解析url。使用 [file-loader](//loaders/file-loader.md) 发射extract-loader的结果作为单独的文件。

```javascript
import stylesheetUrl from "file-loader!extract-loader!css-loader!main.css";
// stylesheetUrl 将是最终样式表的哈希url。
```

extract-loader的工作原理类似于 [extract-text-webpack-plugin](/Plugins/third-party/ExtractTextWebpackPlugin.md) ，它作为此插件的替代方案。在计算源代码时，它提供了一个假上下文，该上下文专门用于处理 [html-loader](//loaders/html-loader.md)  或 [css-loader ](//loaders/css-loader.md)生成的代码。因此，它可能在其他情况下不起作用。

<br>

安装
------------------------------------------------------------------------

`npm install extract-loader`

<br>

示例
------------------------------------------------------------------------

### [提取一个 main.css](https://github.com/peerigon/extract-loader/tree/master/examples/main-css)

将CSS与webpack打包在一起有一些好处，比如在开发中引用带有散列url的图像和字体或[模块热替换](http://webpack.github.io/docs/hot-module-replacement-with-webpack.html) 。另一方面，在生产环境中，根据JS的执行来应用样式表不是一个好主意。渲染可能会被延迟，甚至可能出现一个[FOUC(无样式内容闪烁)](https://en.wikipedia.org/wiki/Flash\_of\_unstyled_content)。因此，在最终的生产构建中，将它们作为单独的文件保存还是更好的。

使用extract-loader，您可以引用您的`main.css`作为正则`entry`。以下`webpack.config.js`展示了如何在开发中使用 [style-loader](/loaders/style-loader.md) 加载样式，在生产中将其作为单独的文件。

```javascript
const live = process.env.NODE_ENV === "production";
const mainCss = ["css-loader", path.join(__dirname, "app", "main.css")];

if (live) {
    mainCss.unshift("file-loader?name=[name].[ext]", "extract-loader");
} else {
    mainCss.unshift("style-loader");
}

module.exports = {
    entry: [
        path.join(__dirname, "app", "main.js"),
        mainCss.join("!")
    ],
    ...
};
```

### [提取 index.html](https://github.com/peerigon/extract-loader/tree/master/examples/index-html)

你甚至可以添加`index.html`作为`entry`，只从那里引用您的样式表。您只需要告诉html-loader也要获取`link:href`:

```javascript
const indexHtml = path.join(__dirname, "app", "index.html");

module.exports = {
    entry: [
        path.join(__dirname, "app", "main.js"),
        indexHtml
    ],
    ...
    module: {
        rules: [
            {
                test: indexHtml,
                use: [
                    {
                        loader: "file-loader",
                        options: {
                            name: "[name]-dist.[ext]",
                        },
                    },
                    {
                        loader: "extract-loader",
                    },
                    {
                        loader: "html-loader",
                        options: {
                            attrs: ["img:src", "link:href"],
                            interpolate: true,
                        },
                    },
                ],
            },
            {
                test: /\.css$/,
                loaders: [
                    {
                        loader: "file-loader",
                    },
                    {
                        loader: "extract-loader",
                    },
                    {
                        loader: "css-loader",
                    },
                ],
            },
            {
                test: /\.jpg$/,
                loaders: [
                    {
                        loader: "file-loader"
                    },
                ],
            },
        ]
    }
};
```

转换前：

```html
<html>
<head>
    <link href="main.css" type="text/css" rel="stylesheet">
</head>
<body>
    <img src="hi.jpg">
</body>
</html>
```

转换后

```html
<html>
<head>
    <link href="7c57758b88216530ef48069c2a4c685a.css" type="text/css" rel="stylesheet">
</head>
<body>
    <img src="6ac05174ae9b62257ff3aa8be43cf828.jpg">
</body>
</html>
```

<br>

选项
------------------------------------------------------------------------


目前有一种选项:`publicPath`。
如果您正在webpack的[output 选项](//configuration/output.md#outputpublicpath)中使用相对`publicPath`，并将其提取到 [file-loader](//loaders/file-loader.md) 中，那么您可能需要它来设置提取出来的文件的位置。

Example:

```js
module.exports = {
    output: {
        path: path.resolve("./dist"),
        publicPath: "dist/"
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    {
                        loader: "file-loader",
                        options: {
                            name: "assets/[name].[ext]",
                        },
                    },
                    {
                        loader: "extract-loader",
                        options: {
                            publicPath: "../",
                        }
                    },
                    {
                        loader: "css-loader",
                    },
                ],
            }
        ]
    }
};
```

你需要另一个选项?那么你应该考虑一下:

<br>

贡献
------------------------------------------------------------------------

From opening a bug report to creating a pull request: **every contribution is appreciated and welcome**. If you're planing to implement a new feature or change the api please create an issue first. This way we can ensure that your precious work is not in vain.

All pull requests should have 100% test coverage (with notable exceptions) and need to pass all tests.

- Call `npm test` to run the unit tests
- Call `npm run coverage` to check the test coverage (using [istanbul](https://github.com/gotwarlost/istanbul))

<br>

License
------------------------------------------------------------------------

Unlicense

Sponsors
------------------------------------------------------------------------

[<img src="https://assets.peerigon.com/peerigon/logo/peerigon-logo-flat-spinat.png" width="150" />](https://peerigon.com)
