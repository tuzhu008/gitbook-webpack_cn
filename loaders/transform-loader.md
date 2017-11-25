[![npm][npm]][npm-url]
[![deps][deps]][deps-url]
[![chat][chat]][chat-url]

<div align="center">
  <!-- replace with accurate logo e.g from https://worldvectorlogo.com/ -->
  <a href="https://github.com/webpack/webpack">
    <img width="200" height="200" vspace="" hspace="25"
      src="https://webpack.js.org/assets/icon-square-big.svg">
  </a>
  <h1>Transform Loader</h1>
  <p>Use <a href="https://github.com/substack/node-browserify/wiki/list-of-transforms">browserify transforms</a> as webpack-loader.<p>
  <a href="https://github.com/webpack-contrib/transform-loader"><img src="https://img.shields.io/badge/Github-%E6%9F%A5%E7%9C%8B%E6%9B%B4%E5%A4%9A-brightgreen.svg"></a>
</div>

<h2 align="center">安装</h2>

```bash
npm i transform-loader --save
```

<h2 align="center"><a href="https://webpack.js.org/concepts/loaders">用法</a></h2>

将模块名称作为查询参数传递。

``` javascript
var x = require("!transform-loader?brfs!./file.js");
var x = require("!transform-loader/cacheable?brfs!./file.js"); // 缓存版本
```

如果你传递的是一个数字，它就会从`this.options.transforms[number]` 中得到这个函数。

<h2 align="center">webpack 2 配置示例</h2>

``` javascript
module.exports = {
  module: {
    rules: [
      {
        loader: "transform-loader?brfs",
        enforce: "post",
        options: {
          transforms: [
              function (/*file*/) {
                  return through((buffer) => {
                      return this.queue(
                          buffer.split('')
                              .map((chunk) => String.fromCharCode(127-chunk.charCodeAt(0))))
                              .join('')
                  }, () => this.queue(null))
              }
          ]
        }
      },

      {
        test: /\.coffee$/,
        loader: "transform-loader/cacheable?coffeeify",
        options: {
          transforms: [
              function (/*file*/) {
                  return through((buffer) => {
                      return this.queue(
                          buffer.split('')
                              .map((chunk) => String.fromCharCode(127-chunk.charCodeAt(0))))
                              .join('')
                  }, () => this.queue(null))
              }
          ]
        }
      },

      {
        test: /\.weirdjs$/,
        loader: "transform-loader?0",
        options: {
          transforms: [
              function (/*file*/) {
                  return through((buffer) => {
                      return this.queue(
                          buffer.split('')
                              .map((chunk) => String.fromCharCode(127-chunk.charCodeAt(0))))
                              .join('')
                  }, () => this.queue(null))
              }
          ]
        }
      }
    ]
  }
};
```

<h2 align="center">webpack 1 配置示例</h2>

``` javascript
module.exports = {
	module: {
		postLoaders: [
			{
				loader: "transform-loader?brfs"
			}
		]
		loaders: [
			{
				test: /\.coffee$/,
				loader: "transform-loader/cacheable?coffeeify"
			},
			{
				test: /\.weirdjs$/,
				loader: "transform-loader?0"
			}
		]
	},
	transforms: [
		function(file) {
			return through(function(buf) {
				this.queue(buf.split("").map(function(s) {
					return String.fromCharCode(127-s.charCodeAt(0));
				}).join(""));
			}, function() { this.queue(null); });
		}
	]
};
```

<h2 align="center">典型的 brfs 示例</h2>

假设您有以下 Node 源:

```js
var test = require('fs').readFileSync('./test.txt', 'utf8');
```

After 在运行 `npm install transform-loader brfs --save` 安装完成之后，, 添加下面的loader到配置中:

```js
module.exports = {
    context: __dirname,
    entry: "./index.js",
    module: {
        loaders: [
            {
                test: /\.js$/,
                loader: "transform-loader?brfs"
            }
        ]
    }
}
```

这个 loader 被应用到所有的 JS 文件，这可能会导致观察任务的性能下降。所以可以使用 `transform-loader/cacheable?brfs` 代替。 

<h2 align="center">维护者</h2>

<table>
  <tbody>
    <tr>
      <td align="center">
        <img width="150" height="150"
        src="https://avatars3.githubusercontent.com/u/166921?v=3&s=150">
        </br>
        <a href="https://github.com/bebraw">Juho Vepsäläinen</a>
      </td>
      <td align="center">
        <img width="150" height="150"
        src="https://avatars2.githubusercontent.com/u/8420490?v=3&s=150">
        </br>
        <a href="https://github.com/d3viant0ne">Joshua Wiens</a>
      </td>
      <td align="center">
        <img width="150" height="150"
        src="https://avatars3.githubusercontent.com/u/533616?v=3&s=150">
        </br>
        <a href="https://github.com/SpaceK33z">Kees Kluskens</a>
      </td>
      <td align="center">
        <img width="150" height="150"
        src="https://avatars3.githubusercontent.com/u/3408176?v=3&s=150">
        </br>
        <a href="https://github.com/TheLarkInn">Sean Larkin</a>
      </td>
    </tr>
  <tbody>
</table>


[npm]: https://img.shields.io/npm/v/transform-loader.svg
[npm-url]: https://npmjs.com/package/transform-loader

[deps]: https://david-dm.org/webpack-contrib/transform-loader.svg
[deps-url]: https://david-dm.org/webpack-contrib/transform-loader

[chat]: https://img.shields.io/badge/gitter-webpack%2Fwebpack-brightgreen.svg
[chat-url]: https://gitter.im/webpack/webpack
