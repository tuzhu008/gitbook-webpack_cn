[![npm][npm]][npm-url]
[![node][node]][node-url]
[![deps][deps]][deps-url]
[![tests][tests]][tests-url]
[![coverage][cover]][cover-url]
[![chat][chat]][chat-url]

<div align="center">
  <img width="160" height="160"
    src="https://cdn.worldvectorlogo.com/logos/coffeescript.svg">
  <a href="https://github.com/webpack/webpack">
    <img width="200" height="200" hspace="20"
      src="https://webpack.js.org/assets/icon-square-big.svg">
  </a>
  
</div>

<h1>Coffee Loader</h1>

<p>像 JavaScripty 一样加载<a href="http://coffeescript.org/"> CoffeeScript</a></p>

[![](https://img.shields.io/badge/Github-%E6%9F%A5%E7%9C%8B%E6%9B%B4%E5%A4%9A-brightgreen.svg)
](https://github.com/webpack-contrib/coffee-loader)



<h2>安装</h2>

```bash
npm install --save-dev coffee-loader
```

<h2>用法</h2>


```js
import coffee from 'coffee-loader!./file.coffee';
```

### `配置 (推荐)`


```js
import coffee from 'file.coffee';
```

**webpack.config.js**
```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.coffee$/,
        use: [ 'coffee-loader' ]
      }
    ]
  }
}
```

<h2 align="center">Options</h2>

|姓名|默认值|描述|
|:--:|:-----:|:----------|
|**`literate`**|`false`| 在 Markdown 中启用CoffeeScript(代码块) 比如 `file.coffee.md`|
|**`sourceMap`**|`false`|启用/禁用 Sourcemaps|
|**`transpile`**|`false`|提供 Babel presets 和 plugins|

### [`Literate`](http://coffeescript.org/#literate)

**webpack.config.js**
```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.coffee.md$/,
        use: [
          {
            loader: 'coffee-loader',
            options: { literate: true }
          }
        ]
      }
    ]
  }
}
```

### `Sourcemaps`

**webpack.config.js**
```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.coffee$/,
        use: [
          {
            loader: 'coffee-loader',
            options: { sourceMap: true }
          }
        ]
      }
    ]
  }
}
```

### [`Transpile`](http://coffeescript.org/#transpilation)

**webpack.config.js**
```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.coffee$/,
        use: [
          {
            loader: 'coffee-loader',
            options: { 
              transpile: {
                presets: ['env']
              }
            }
          }
        ]
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


[npm]: https://img.shields.io/npm/v/coffee-loader.svg
[npm-url]: https://npmjs.com/package/coffee-loader

[node]: https://img.shields.io/node/v/coffee-loader.svg
[node-url]: https://nodejs.org

[deps]: https://david-dm.org/webpack/coffee-loader.svg
[deps-url]: https://david-dm.org/webpack/coffee-loader

[tests]: http://img.shields.io/travis/webpack/coffee-loader.svg
[tests-url]: https://travis-ci.org/webpack/coffee-loader

[cover]: https://coveralls.io/repos/github/webpack/coffee-loader/badge.svg
[cover-url]: https://coveralls.io/github/webpack/coffee-loader

[chat]: https://badges.gitter.im/webpack/webpack.svg
[chat-url]: https://gitter.im/webpack/webpack
