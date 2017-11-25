[![npm][npm]][npm-url]
[![deps][deps]][deps-url]
[![chat][chat]][chat-url]

<div align="center">
  <!-- replace with accurate logo e.g from https://worldvectorlogo.com/ -->
  <a href="https://github.com/webpack/webpack">
    <img width="200" height="200" vspace="" hspace="25"
      src="https://webpack.js.org/assets/icon-square-big.svg">
  </a>
  <h1>Null Loader</h1>
  <p>返回空模块的loader。<p>
  <a href="https://github.com/webpack-contrib/null-loader"><img src="https://img.shields.io/badge/Github-查看更多-brightgreen.svg"></a>
</div>

<h2 align="center">安装</h2>

```bash
npm i null-loader --save
```

<h2 align="center">示例</h2>

这个loader的一个用途是让依赖项导入的模块静默(silence)。例如，您的项目依赖于一个ES6库，它导入一个您不需要的polyfill，因此删除它将不会导致功能的损失。测试（在`test`中设置）polyfill的路径，它不会包含在你的包中:

```js
const path = require('path');

module.exports = {
  module: {
    rules: [
      {
        test: path.resolve(__dirname, 'node_modules/library/polyfill.js'),
        use: 'null-loader'
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


[npm]: https://img.shields.io/npm/v/null-loader.svg
[npm-url]: https://npmjs.com/package/null-loader

[deps]: https://david-dm.org/webpack-contrib/null-loader.svg
[deps-url]: https://david-dm.org/webpack-contrib/null-loader

[chat]: https://img.shields.io/badge/gitter-webpack%2Fwebpack-brightgreen.svg
[chat-url]: https://gitter.im/webpack/webpack
