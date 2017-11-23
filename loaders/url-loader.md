
[![npm][npm]][npm-url]
[![node][node]][node-url]
[![deps][deps]][deps-url]
[![tests][tests]][tests-url]
[![coverage][cover]][cover-url]
[![chat][chat]][chat-url]

<div align="center">
  <a href="https://github.com/webpack/webpack">
    <img width="200" height="200"
      src="https://cdn.rawgit.com/webpack/media/e7485eb2/logo/icon.svg">
  </a>
</div>
<h1>URL Loader</h1>

[![github]][github-url]

加载文件为`base64`编码的URL

<h2>安装</h2>

```bash
npm install --save-dev url-loader
```

<h2>用法</h2>

`url-loader` 功能类似于 [`file-loader`](https://github.com/webpack-contrib/file-loader)，但是在文件大小（单位 byte）低于指定的限制时，可以返回一个 DataURL。

```js
import img from './image.png'
```

**webpack.config.js**
```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.(png|jpg|gif)$/,
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 8192
            }
          }
        ]
      }
    ]
  }
}
```

<h2 align="center">Options</h2>

|Name|Type|Default|Description|
|:--:|:--:|:-----:|:----------|
|**`limit`**|`{Number}`|`undefined`|作为Data URL的内联文件的字节限制|
|**`mimetype`**|`{String}`|`extname`|为文件指定 MIME 类型(否则就会从文件扩展中推断出来)|
|**`fallback`**|`{String}`|`file-loader`|当文件超过限制时，为文件指定`loader` |

### `limit`

如果文件超过限制，默认情况下[`file-loader`](https://github.com/webpack-contrib/file-loader)将被使用，并且所有的查询参数都会被传递给它。
也可以通过`fallback`选项使用其他的loader。

这个显示可以通过loader选项来指定，默认情况下无限制。

**webpack.config.js**
```js
{
  loader: 'url-loader',
  options: {
    limit: 8192
  }
}
```

### `mimetype`

为文件设置 MIME。如果未指定，文件扩展名将被用于查找MIME类型。

**webpack.config.js**
```js
{
  loader: 'url-loader',
  options: {
    mimetype: 'image/png'
  }
}
```

### `fallback`

**webpack.config.js**
```js
{
  loader: 'url-loader',
  options: {
    fallback: 'responsive-loader'
  }
}
```

<h2 >维护者</h2>

<table>
  <tbody>
    <tr>
      <td align="center">
        <a href="https://github.com/bebraw">
          <img width="150" height="150" src="https://github.com/bebraw.png?v=3&s=150">
          </br>
          Juho Vepsäläinen
        </a>
      </td>
      <td align="center">
        <a href="https://github.com/d3viant0ne">
          <img width="150" height="150" src="https://github.com/d3viant0ne.png?v=3&s=150">
          </br>
          Joshua Wiens
        </a>
      </td>
      <td align="center">
        <a href="https://github.com/sapegin">
          <img width="150" height="150" src="https://github.com/sapegin.png?v=3&s=150">
          </br>
          Artem Sapegin
        </a>
      </td>
      <td align="center">
        <a href="https://github.com/michael-ciniawsky">
          <img width="150" height="150" src="https://github.com/michael-ciniawsky.png?v=3&s=150">
          </br>
          Michael Ciniawsky
        </a>
      </td>
      <td align="center">
        <a href="https://github.com/evilebottnawi">
          <img width="150" height="150" src="https://github.com/evilebottnawi.png?v=3&s=150">
          </br>
          Alexander Krasnoyarov
        </a>
      </td>
    </tr>
  <tbody>
</table>


[github]: https://img.shields.io/badge/Github-%E6%9F%A5%E7%9C%8B%E6%9B%B4%E5%A4%9A-brightgreen.svg
[github-url]: https://github.com/webpack-contrib/url-loader

[npm]: https://img.shields.io/npm/v/url-loader.svg
[npm-url]: https://npmjs.com/package/url-loader

[node]: https://img.shields.io/node/v/url-loader.svg
[node-url]: https://nodejs.org

[deps]: https://david-dm.org/webpack-contrib/url-loader.svg
[deps-url]: https://david-dm.org/webpack-contrib/url-loader

[tests]: http://img.shields.io/travis/webpack-contrib/url-loader.svg
[tests-url]: https://travis-ci.org/webpack-contrib/url-loader

[cover]: https://coveralls.io/repos/github/webpack-contrib/url-loader/badge.svg
[cover-url]: https://coveralls.io/github/webpack-contrib/url-loader

[chat]: https://badges.gitter.im/webpack/webpack.svg
[chat-url]: https://gitter.im/webpack/webpack
