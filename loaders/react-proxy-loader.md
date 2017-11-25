[![npm][npm]][npm-url]
[![deps][deps]][deps-url]
[![chat][chat]][chat-url]

<div align="center">
  <!-- replace with accurate logo e.g from https://worldvectorlogo.com/ -->
  <a href="https://github.com/webpack/webpack">
    <img width="200" height="200" vspace="" hspace="25"
      src="https://webpack.js.org/assets/icon-square-big.svg">
  </a>
  <h1>React Proxy Loader</h1>
  <p>将 React 组件封装到代理（proxy）组件中，以启用代码分割。(按需加载 react 组件及其依赖 ).<p>
  <a href="https://github.com/webpack-contrib/react-proxy-loader"><img src="https://img.shields.io/badge/Github-查看更多-brightgreen.svg"></a>
</div>

<h2 align="center">安装</h2>

```bash
npm install react-proxy-loader
```

<h2 align="center"><a href="https://webpack.js.org/concepts/loaders">用法</a></h2>

``` js
var Component = require("react-proxy-loader!./Component");
// => 返回代理组件（它按需加载。）
// (webpack 为此组件及其依赖项创建一个额外的 chunk)

var ComponentProxyMixin = require("react-proxy-loader!./Component").Mixin;
// => 返回代理组件的 mixin
// (这允许您为 proxy 的加载状态设置渲染)
var ComponentProxy = React.createClass({
	mixins: [ComponentProxyMixin],
	renderUnavailable: function() {
		return <p>Loading...</p>;
	}
});
```

代理是一个react组件。所有属性都将传输到包装组件。

<h2 align="center">配置</h2>

代替（或除了）内联 loader 调用之外，还可以在配置中指定代理组件：

``` js
module.exports = {
	module: {
		loaders: [
			/* ... */
			{
				test: [
					/component\.jsx$/, // 依靠 RegExp 选择组件
					/\.async\.jsx$/, // 依靠扩展选择组件

					"/abs/path/to/component.jsx" // 组件的绝对路径
				],
				loader: "react-proxy-loader"
			}
		]
	}
};
```

### Chunk name

你可以使用`name`查询参数为chunk命名

``` js
var Component = require("react-proxy-loader?name=chunkName!./Component");
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


[npm]: https://img.shields.io/npm/v/react-proxy-loader.svg
[npm-url]: https://npmjs.com/package/react-proxy-loader

[deps]: https://david-dm.org/webpack-contrib/react-proxy-loader.svg
[deps-url]: https://david-dm.org/webpack-contrib/react-proxy-loader

[chat]: https://img.shields.io/badge/gitter-webpack%2Fwebpack-brightgreen.svg
[chat-url]: https://gitter.im/webpack/webpack
