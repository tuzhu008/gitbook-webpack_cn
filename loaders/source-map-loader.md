[![npm][npm]][npm-url]
[![deps][deps]][deps-url]
[![chat][chat]][chat-url]

<div align="center">
  <!-- replace with accurate logo e.g from https://worldvectorlogo.com/ -->
  <a href="https://github.com/webpack/webpack">
    <img width="200" height="200" vspace="" hspace="25"
      src="https://webpack.js.org/assets/icon-square-big.svg">
  </a>
  <h1>Sourcemap Loader</h1>
  <p>从现有源文件中提取源映射 (从它们的 <code>sourceMappingURL</code>).<p>
  <a href="https://github.com/webpack-contrib/source-map-loader"><img src="https://img.shields.io/badge/Github-%E6%9F%A5%E7%9C%8B%E6%9B%B4%E5%A4%9A-brightgreen.svg"></a>
</div>

<h2 align="center">安装</h2>

```bash
npm i -D source-map-loader
```

<h2 align="center">用法</h2>

[Documentation: Using loaders](https://webpack.js.org/concepts/#loaders)


### 示例 webpack 配置

``` javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        use: ["source-map-loader"],
        enforce: "pre"
      }
    ]
  }
};
```

`source-map-loader`从所有的 JavaScript 入口中提取现有的源映射。这包括内联源映射和通过URL链接的内容。所有的源映射数据都被传递给webpack，以作为在[webpack.config.js](//configuration/README.md)中由 `devtool` 选项指定的每一个选择的[源映射样式](//configuration/devtools.md)进行处理。

当使用的第三方库拥有自己的源映射时，这个 loader 特别有用。如果不提取和处理 webpack bundle 的源映射，浏览器可能会曲解源映射数据。`source-map-loader` 允许 webpack 在整个库中维护源映射数据的连续性，因此依然易于进行调试。

`source-map-loader`从任何 JavaScript 文件中进行提取，包括 `node_modules` 目录中的那些文件。注意设置[include](//configuration/module.md#ruleinclude) 和 [exclude](//configuration/module.md#ruleexclude)规则条件，以最大化打包性能。
  

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


[npm]: https://img.shields.io/npm/v/source-map-loader.svg
[npm-url]: https://npmjs.com/package/source-map-loader

[deps]: https://david-dm.org/webpack-contrib/source-map-loader.svg
[deps-url]: https://david-dm.org/webpack-contrib/source-map-loader

[chat]: https://img.shields.io/badge/gitter-webpack%2Fwebpack-brightgreen.svg
[chat-url]: https://gitter.im/webpack/webpack
