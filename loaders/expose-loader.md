[![npm][npm]][npm-url]
[![deps][deps]][deps-url]
[![chat][chat]][chat-url]

<div align="center">
  <!-- replace with accurate logo e.g from https://worldvectorlogo.com/ -->
  <a href="https://github.com/webpack/webpack">
    <img width="200" height="200" vspace="" hspace="25"
      src="https://webpack.js.org/assets/icon-square-big.svg">
  </a>
  <h1>Expose Loader</h1>
  <p>expose loader将模块添加到全局对象中。这对于调试非常有用，或者 <a href="https://webpack.js.org/guides/shimming/">支持依赖于全局库的库</a>.<p>
  <a href="https://github.com/webpack-contrib/expose-loader"><img src="https://img.shields.io/badge/Github-%E6%9F%A5%E7%9C%8B%E6%9B%B4%E5%A4%9A-brightgreen.svg"></a>
</div>

<h2 align="center">安装</h2>

```bash
npm i expose-loader --save
```

<h2 align="center"><a href="https://webpack.js.org/concepts/loaders">用法</a></h2>

> **[info]** 注：
>
> 模块必须在您的bundle内被`require()`，否则它们不会被暴露。

``` javascript
require("expose-loader?libraryName!./file.js");
// 将file.js的出口暴露给全局上下文的属性`libraryName`.
// 在浏览器中，window.libraryName是可用的。
```

例如，假设您希望将jQuery暴露为一个全局变量`$`:

```js
require("expose-loader?$!jquery");
```

因此,`window.$`然后在浏览器控制台中是可用的。

另外，您可以将其设置为配置文件:

webpack v1 用法
```js
module: {
  loaders: [
    { test: require.resolve("jquery"), loader: "expose-loader?$" }
  ]
}
```
webpack v2 用法
```js
module: {
  rules: [{
          test: require.resolve('jquery'),
          use: [{
              loader: 'expose-loader',
              options: '$'
          }]
      }]
}
```

假设除了`window.$`还希望将它暴露为`window.jQuery`。

你可以在loader字符串中使用`!`来expose多个:

webpack v1 usage
```js
module: {
  loaders: [
    { test: require.resolve("jquery"), loader: "expose-loader?$!expose-loader?jQuery" },
  ]
}
```
webpack v2 usage
```js
module: {
  rules: [{
          test: require.resolve('jquery'),
          use: [{
              loader: 'expose-loader',
              options: 'jQuery'
          },{
              loader: 'expose-loader',
              options: '$'
          }]
      }]
}
```

[`require.resolve`](https://nodejs.org/api/all.html#modules_require_resolve)
是一个Node.js 调用(与webpack处理的 `require.resolve` 不相关). `require.resolve` 给出了这个模块的绝对路径(`"/.../app/node_modules/react/react.js"`)。
因此，expose 只适用于React模块。它只在bundle中使用时才会被暴露出来。

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


[npm]: https://img.shields.io/npm/v/expose-loader.svg
[npm-url]: https://npmjs.com/package/expose-loader

[deps]: https://david-dm.org/webpack-contrib/expose-loader.svg
[deps-url]: https://david-dm.org/webpack-contrib/expose-loader

[chat]: https://img.shields.io/badge/gitter-webpack%2Fwebpack-brightgreen.svg
[chat-url]: https://gitter.im/webpack/webpack
