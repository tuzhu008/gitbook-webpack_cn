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
  <h1>Bundle Loader</h1>
  <p>Bundle loader for webpack<p>
  <a href="https://github.com/webpack-contrib/bundle-loader"><img src="https://img.shields.io/badge/Github-%E6%9F%A5%E7%9C%8B%E6%9B%B4%E5%A4%9A-brightgreen.svg"></a>
</div>

<h2 align="center">安装</h2>

```bash
npm i bundle-loader --save
```

<h2 align="center"><a href="https://webpack.js.org/concepts/loaders">用法</a></h2>

**webpack.config.js**
```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.bundle\.js$/,
        use: 'bundle-loader'
      }
    ]
  }
}
```

当需要这个bundle，请求这个chunk

**file.js**
```js
import bundle from './file.bundle.js';
```

等待chunk可用(并获得导出)，您需要异步等待它。

```js
bundle((file) => {
  // 就像文件被require一样使用它。
  const file = require('./file.js')
});
```

包装`require('file.js')`在`require.ensure`块中。

可以添加多个回调。他们将按照添加的顺序被执行。

```js
bundle(callbackTwo)
bundle(callbackThree)
```

如果在加载了依赖项之后添加了一个回调，那么将立即调用它。

<h2 align="center">选项</h2>

|名称|类型|默认值|描述|
|:--:|:--:|:-----:|:----------|
|**`lazy`**|`{Boolean}`|`false`|异步加载导入的包|
|**`name`**|`{String}`|`[id].[name]`| 为导入的包配置一个自定义的文件名 |

### `lazy`

当require `bundle-loader`时，这个文件将被请求。如果想要懒加载它，使用：

**webpack.config.js**
```js
{
  loader: 'bundle-loader',
  options: {
    lazy: true
  }
}
```

```js
import bundle from './file.bundle.js'

bundle((file) => {...})
```

> **[info]** 
> 在调用函数之前，chunk不会被请求。

### `name`

使用`name`选项参数为bundle设置名字。
参见 [文档](https://github.com/webpack/loader-utils#interpolatename).

**webpack.config.js**
```js
{
  loader: 'bundle-loader',
  options: {
    name: '[name]'
  }
}
```

> **[warning]** 警告:
> 
> loader创建chunk，会根据[`output.chunkFilename`](//configuration/output/#output-chunkfilename) 规则对它命名, 默认为 `[id].[name]`。 这里的`[name]`对应于`name`选项参数中设置的chunk名。

<h2 align="center">示例</h2>

```js
import bundle from './file.bundle.js'
```

**webpack.config.js**
``` js
module.exports = {
  entry: {
   index: './App.js'
  },
  output: {
    path: path.resolve(__dirname, 'dest'),
    filename: '[name].js',
    // 或者其他格式
    chunkFilename: '[name].[id].js',
  },
  module: {
    rules: [
      {
        test: /\.bundle\.js$/,
        use: {
          loader: 'bundle-loader',
          options: {
            name: 'my-chunk'
          }
        }
      }
    ]
  }
}
```
普通的chunk将使用上面的`filename`规则显示，并根据它们的`[chunkname]`来命名。

但是，来自`bundle-loader`的chunk，将会使用`chunkFilename`规则加载，所以示例文件将分别生成`my-chunk.1.js`和`file-2.js`。

由于在bundle选项参数中添加`[hash]`不能正确工作，您还可以使用`chunkFilename`来为文件名添加哈希值。

<h2 align="center">维护者</h2>

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


[npm]: https://img.shields.io/npm/v/bundle-loader.svg
[npm-url]: https://npmjs.com/package/bundle-loader

[node]: https://img.shields.io/node/v/bundle-loader.svg
[node-url]: https://nodejs.org

[deps]: https://david-dm.org/webpack-contrib/bundle-loader.svg
[deps-url]: https://david-dm.org/webpack-contrib/bundle-loader

[tests]: http://img.shields.io/travis/webpack-contrib/bundle-loader.svg
[tests-url]: https://travis-ci.org/webpack-contrib/bundle-loader

[cover]: https://coveralls.io/repos/github/webpack-contrib/bundle-loader/badge.svg
[cover-url]: https://coveralls.io/github/webpack-contrib/bundle-loader

[chat]: https://badges.gitter.im/webpack/webpack.svg
[chat-url]: https://gitter.im/webpack/webpack
