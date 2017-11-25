[![npm][npm]][npm-url]
[![deps][deps]][deps-url]
[![test][test]][test-url]
[![coverage][cover]][cover-url]
[![chat][chat]][chat-url]

<div align="center">
  <!-- replace with accurate logo e.g from https://worldvectorlogo.com/ -->
  <a href="https://github.com/webpack/webpack">
    <img width="200" height="200" vspace="" hspace="25"
      src="https://webpack.js.org/assets/icon-square-big.svg">
  </a>
  <h1>SVG Inline Loader for Webpack</h1>
  <p>这个 Webpack loader 将 SVG 作为模块进行内联。如果您使用Adobe 套件或 Sketch 来导出 SVG，您将得到自动生成的、不需要的外壳。这个加载程序也为您删除了它。<p>
  <a href="https://github.com/webpack-contrib/svg-inline-loader"><img src="https://img.shields.io/badge/Github-%E6%9F%A5%E7%9C%8B%E6%9B%B4%E5%A4%9A-brightgreen.svg"></a>
</div>

<h2 align="center">安装</h2>

```bash
npm install svg-inline-loader --save-dev
```

<h2 align="center">配置</h2>

简单地添加配置对象到 `module.loaders`，像这样

```javascript
    {
        test: /\.svg$/,
        loader: 'svg-inline-loader'
    }
```

> **[warning]** 警告：
>
>你应该只通过 `module.loaders` 或 `require('!...')` 配置此 loader 一次。参见 [#15](https://github.com/webpack-contrib/svg-inline-loader/issues/15)获取更多细节。

<h2 align="center">查询选项</h2>

#### `removeTags: boolean`

删除指定的标记及其子元素。您可以通过设置 `removingTags` 查询数组来指定标记。

默认值: `removeTags: false`

#### `removingTags: [...string]`

> **[warning]** 警告：
>
> 这个选项只有在 `removeTags: true` 时才可用。

默认值: `removingTags: ['title', 'desc', 'defs', 'style']`

#### `warnTags: [...string]`

警告标签, 例如: ['desc', 'defs', 'style']

默认: `warnTags: []`

#### `removeSVGTagAttrs: boolean`

从 `<svg />` 移除 `width` 和 `height` 属性。

默认值: `removeSVGTagAttrs: true`

#### `removingTagAttrs: [...string]`

从 `<svg />` 中移除属性。

默认: `removingTagAttrs: []`

#### `warnTagAttrs: [...string]`

在控制台发出关于内部 `<svg />` 属性的警告

默认值: `warnTagAttrs: []`
#### `classPrefix: boolean || string`

添加一个前缀到 svg 文件的 clas s名，以避免碰撞。

默认值: `classPrefix: false`

#### `idPrefix: boolean || string`

添加一个前缀到 svg 文件的id，以避免碰撞。

default: `idPrefix: false`

<h2 align="center">示例用法</h2>

```js
// 使用默认的哈希前缀 (__[hash:base64:7]__)
var logoTwo = require('svg-inline-loader?classPrefix!./logo_two.svg');

// 使用自定义字符串
var logoOne = require('svg-inline-loader?classPrefix=my-prefix-!./logo_one.svg');

// 使用自定义字符串和哈希
var logoThree = require('svg-inline-loader?classPrefix=__prefix-[sha512:hash:hex:5]__!./logo_three.svg');
```
参见 [loader-utils](https://github.com/webpack/loader-utils#interpolatename) 查看 hash 选项。

更好的用法是通过 `module.loaders` :
```js
    {
        test: /\.svg$/,
        loader: 'svg-inline-loader?classPrefix'
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

[npm]: https://img.shields.io/npm/v/svg-inline-loader.svg
[npm-url]: https://npmjs.com/package/svg-inline-loader

[deps]: https://david-dm.org/webpack-contrib/svg-inline-loader.svg
[deps-url]: https://david-dm.org/webpack-contrib/svg-inline-loader

[chat]: https://img.shields.io/badge/gitter-webpack%2Fwebpack-brightgreen.svg
[chat-url]: https://gitter.im/webpack/webpack

[test]: https://travis-ci.org/webpack-contrib/svg-inline-loader.svg?branch=master
[test-url]: https://travis-ci.org/webpack-contrib/svg-inline-loader

[cover]: https://codecov.io/gh/webpack-contrib/svg-inline-loader/branch/master/graph/badge.svg
[cover-url]: https://codecov.io/gh/webpack-contrib/svg-inline-loader
