[![npm][npm]][npm-url]
[![node][node]][node-url]
[![deps][deps]][deps-url]
[![tests][tests]][tests-url]
[![coverage][cover]][cover-url]
[![chat][chat]][chat-url]

<div align="center">
  <a href="https://github.com/webpack/webpack">
    <img width="200" height="200"
      src="https://webpack.js.org/assets/icon-square-big.svg">
  </a>
</div>
<h1>File Loader</h1>

[![github]][github-url]

指示webpack发出所需的对象作为文件，并返回它的公共URL


<h2 >安装</h2>

```bash
npm install --save-dev file-loader
```

<h2>用法</h2>

默认情况下，生成的文件的文件名就是文件内容的 MD5 哈希值，并会保留所请求资源的原始扩展名。

```js
import img from './file.png'
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
            loader: 'file-loader',
            options: {}  
          }
        ]
      }
    ]
  }
}
```

发射 `file.png` 作为一个文件到输出目录，并返回一个公共路径。

```js
"/public/path/0dcbbaa7013869e351f.png"
// 文件名为hash值
// 扩展名与原始文件保持一致
```

<h2 >选项</h2>

|名称|类型|默认值|描述|
|:--:|:--:|:-----:|:----------|
|**`name`**|`{String\|Function}`|`[hash].[ext]`|为文件配置一个自定义的文件名模板|
|**`context`**|`{String}`|`this.options.context`|配置自定义的文件名上下文, 默认为 `webpack.config.js` [context](https://webpack.js.org/configuration/entry-context/#context)|
|**`publicPath`**|`{String\|Function}`|[`__webpack_public_path__ `](https://webpack.js.org/api/module-variables/#__webpack_public_path__-webpack-specific-)|为文件配置一个自定义 `public` 路径|
|**`outputPath`**|`{String\|Function}`|`'undefined'`|为文件配置一个自定义`output` 路径 |
|**`useRelativePath`**|`{Boolean}`|`false`| 如果你希望为每一个文件生成一个相对于URL的`context`，应该设置为`true` |
|**`emitFile`**|`{Boolean}`|`true`| 默认情况下，文件会被发射，但是需要，可以将它禁用（例如，使用了服务器端的package）|

### `name`


可以使用查询参数`name`为您的文件配置一个自定义的文件名模板。例如，将一个文件从您的`context`目录复制到输出目录中，保留完整的目录结构，可能会使用

#### `{String}`

**webpack.config.js**
```js
{
  loader: 'file-loader',
  options: {
    name: '[path][name].[ext]'
  }  
}
```

#### `{Function}`

**webpack.config.js**
```js
{
  loader: 'file-loader',
  options: {
    name (file) {
      if (env === 'development') {
        return '[path][name].[ext]'
      }

      return '[hash].[ext]'
    }
  }  
}
```

#### `placeholders`

|名称|类型|默认值|描述|
|:--:|:--:|:-----:|:----------|
|**`[ext]`**|`{String}`|`file.extname`| 资源的扩展 |
|**`[name]`**|`{String}`|`file.basename`| 资源的basename |
|**`[path]`**|`{String}`|`file.dirname`|资源相对于 `context`的路径 |
|**`[hash]`**|`{String}`|`md5`| 内容的hash，下面是更多信息 |
|**`[N]`**|`{Number}`|``|`n-th`匹配 从匹配当前文件名与查询参数 `regExp`获得的东西|

#### `hashes`

`[<hashType>:hash:<digestType>:<length>]` 你也可以选择配置

|名称|类型|默认值|描述|
|:--:|:--:|:-----:|:----------|
|**`hashType`**|`{String}`|`md5`|`sha1`, `md5`, `sha256`, `sha512`|
|**`digestType`**|`{String}`|`base64`|`hex`, `base26`, `base32`, `base36`, `base49`, `base52`, `base58`, `base62`, `base64`|
|**`length`**|`{Number}`|`8`|字符的长度|

默认情况下，您指定的路径和名称将在同一个目录中输出该文件，并将使用相同的URL路径来访问该文件。

### `context`

**webpack.config.js**
```js
{
  loader: 'file-loader',
  options: {
    name: '[path][name].[ext]',
    context: ''
  }  
}
```

你可以使用`outputPath`, `publicPath` 和 `useRelativePath`来指定自定义的`output` 和 `public`路径。


### `publicPath`

**webpack.config.js**
```js
{
  loader: 'file-loader',
  options: {
    name: '[path][name].[ext]',
    publicPath: 'assets/'
  }  
}
```

### `outputPath`

**webpack.config.js**
```js
{
  loader: 'file-loader',
  options: {
    name: '[path][name].[ext]',
    outputPath: 'images/'
  }  
}
```

### `useRelativePath`

如果您希望为每个文件上下文生成一个相对URL，`useRelativePath` 应该是`true`

```js
{
  loader: 'file-loader',
  options: {
    useRelativePath: process.env.NODE_ENV === "production"
  }
}
```

### `emitFile`

默认情况下文件是被发射的，但是如果需要，这是可以被禁用的（例如，对于服务器端的package）。

```js
import img from './file.png'
```

```js
{
  loader: 'file-loader',
  options: {
    emitFile: false
  }  
}
```

> ⚠️  返回公共URL，但**不**发出文件

```
`${publicPath}/0dcbbaa701328e351f.png`
```

<h2 >示例</h2>


```js
import png from 'image.png'
```

**webpack.config.js**
```js
{
  loader: 'file-loader',
  options: {
    name: 'dirname/[hash].[ext]'
  }  
}
```

```
dirname/0dcbbaa701328ae351f.png
```

**webpack.config.js**
```js
{
  loader: 'file-loader',
  options: {
    name: '[sha512:hash:base64:7].[ext]'
  }  
}
```

```
gdyb21L.png
```

```js
import png from 'path/to/file.png'
```

**webpack.config.js**
```js
{
  loader: 'file-loader',
  options: {
    name: '[path][name].[ext]?[hash]'
  }  
}
```

```
path/to/file.png?e43b20c069c4a01867c31e98cbce33c9
```

<h2>贡献者</h2>

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

[github]: https://img.shields.io/badge/Github-查看更多-brightgreen.svg
[github-url]: https://github.com/webpack-contrib/file-loader

[npm]: https://img.shields.io/npm/v/file-loader.svg
[npm-url]: https://npmjs.com/package/file-loader

[node]: https://img.shields.io/node/v/file-loader.svg
[node-url]: https://nodejs.org

[deps]: https://david-dm.org/webpack-contrib/file-loader.svg
[deps-url]: https://david-dm.org/webpack-contrib/file-loader

[tests]: http://img.shields.io/travis/webpack-contrib/file-loader.svg
[tests-url]: https://travis-ci.org/webpack-contrib/file-loader

[cover]: https://img.shields.io/codecov/c/github/webpack-contrib/file-loader.svg
[cover-url]: https://codecov.io/gh/webpack-contrib/file-loader

[chat]: https://badges.gitter.im/webpack/webpack.svg
[chat-url]: https://gitter.im/webpack/webpack
