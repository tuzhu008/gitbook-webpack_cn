[![npm][npm]][npm-url]
[![node][node]][node-url]
[![deps][deps]][deps-url]
[![chat][chat]][chat-url]

<div align="center">
  <img width="200" height="200"
    src="https://cdn.worldvectorlogo.com/logos/nodejs-icon.svg">
  <a href="https://github.com/webpack/webpack">
    <img width="200" height="200" hspace="20"
      src="https://webpack.js.org/assets/icon-square-big.svg">
  </a>
  <h1>Node Loader</h1>
  <p>用于 webpack 的 node 附件</p>
   <a href="https://github.com/webpack-contrib/node-loader"><img src="https://img.shields.io/badge/Github-查看更多-brightgreen.svg"></a>
</div>

<h2 align="center">安装</h2>

```bash
npm install --save-dev node-loader
```

<h2 align="center">用法</h2>

执行 [node 附件](https://nodejs.org/dist/latest/docs/api/addons.html) in [enhanced-require](https://github.com/webpack/enhanced-require).

通过您的webpack配置、CLI或内联方式使用loader。

### Via webpack config (recommended)

**webpack.config.js**
```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.node$/,
        use: 'node-loader'
      }
    ]
  }
}
```

**在应用程序中**
```js
import node from 'file.node';
```

### CLI

```bash
webpack --module-bind 'node=node-loader'
```

**在应用程序中**
```js
import node from 'file.node';
```

### 内联


**在应用程序中**
```js
import node from 'node-loader!./file.node';
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


[npm]: https://img.shields.io/npm/v/node-loader.svg
[npm-url]: https://npmjs.com/package/node-loader

[node]: https://img.shields.io/node/v/node-loader.svg
[node-url]: https://nodejs.org

[deps]: https://david-dm.org/webpack/node-loader.svg
[deps-url]: https://david-dm.org/webpack/node-loader

[chat]: https://badges.gitter.im/webpack/webpack.svg
[chat-url]: https://gitter.im/webpack/webpack
