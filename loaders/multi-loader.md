[![npm][npm]][npm-url]
[![deps][deps]][deps-url]
[![chat][chat]][chat-url]

<div align="center">
  <!-- replace with accurate logo e.g from https://worldvectorlogo.com/ -->
  <a href="https://github.com/webpack/webpack">
    <img width="200" height="200" vspace="" hspace="25"
      src="https://webpack.js.org/assets/icon-square-big.svg">
  </a>
  <h1>Multi-Loader</h1>
  <p>这个require一个模块多次，每次加载一个不同的loader。就像在多入口点一样，最后一项的erports被导出。<p>
  <a href="https://github.com/webpack-contrib/multi-loader"><img src="https://img.shields.io/badge/Github-查看更多-brightgreen.svg"></a>
</div>


<h2 align="center">安装</h2>

```bash
npm i multi-loader --save
```

<h2 align="center">用法</h2>

``` javascript
var multi = require("multi-loader");
{
	module: {
		loaders: [
			{
				test: /\.css$/,
				// 添加 CSS 到 DOM
				// 并且
				// 返回未被加工的内容
				loader: multi(
					"style-loader!css-loader!autoprefixer-loader",
					"raw-loader"
				)
			}
		]
	}
}
```

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


[npm]: https://img.shields.io/npm/v/multi-loader.svg
[npm-url]: https://npmjs.com/package/multi-loader

[deps]: https://david-dm.org/webpack-contrib/multi-loader.svg
[deps-url]: https://david-dm.org/webpack-contrib/multi-loader

[chat]: https://img.shields.io/badge/gitter-webpack%2Fwebpack-brightgreen.svg
[chat-url]: https://gitter.im/webpack/webpack
