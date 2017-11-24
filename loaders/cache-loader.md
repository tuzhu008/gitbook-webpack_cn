[![npm][npm]][npm-url]
[![node][node]][node-url]
[![deps][deps]][deps-url]
[![test][test]][test-url]
[![coverage][cover]][cover-url]
[![chat][chat]][chat-url]

<div align="center">
  <a href="https://webpack.js.org/">
    <img width="200" height="200" src="https://webpack.js.org/assets/icon-square-big.svg">
  </a>
  <h1>Cache Loader</h1>
  <p>在磁盘(默认)或数据库中缓存以下loader的结果</p>
  <a href="https://github.com/webpack-contrib/cache-loader"><img src="https://img.shields.io/badge/Github-%E5%8E%9F%E6%96%87%E5%9C%B0%E5%9D%80-brightgreen.svg"></a>
</div>

<h2 align="center">安装</h2>

```bash
npm install --save-dev cache-loader
```

<h2 align="center">用法</h2>

将此loader添加到其他(昂贵的)loader前，以将结果缓存到磁盘上。[译者注：添加到开销大的loader前是因为cache-loader可以缓存这些昂贵loader的结果，之后的loader直接调用缓存，节省开销]

**webpack.config.js**
```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.ext$/,
        use: [
          'cache-loader', //放前面
          ...loaders
        ],
        include: path.resolve('src')
      }
    ]
  }
}
```

> **[info]** 注
> 节省读取和保存缓存文件的开销，所以只需要使用这个loader来缓存昂贵的loader。

<h2 align="center">选项</h2>

|名称|类型|默认值|描述|
|:--:|:--:|:-----:|:----------|
|**`cacheKey`**|`{Function(options, request) -> {String}}`|`undefined`| 允许覆盖默认的高速缓存key生成器 |
|**`cacheDirectory`**|`{String}`|`path.resolve('.cache-loader')`|提供一个缓存目录，也就是缓存项应该被存储的地方 (被用于默认的 read/write 实现)|
|**`cacheIdentifier`**|`{String}`|`cache-loader:{version} {process.env.NODE_ENV}`|提供一个无效标识符，用于生成哈希。您可以使用它作为loader的额外依赖项(被用于默认的 read/write 实现) |
|**`write`**|`{Function(cacheKey, data, callback) -> {void}}`|`undefined`|允许覆盖默认的写入缓存数据到文件(例如. Redis, memcached)|
|**`read`**|`{Function(cacheKey, callback) -> {void}}`|`undefined`|  允许覆盖默认的从文件中读取缓存数据 |

<h2 align="center">示例</h2>

**webpack.config.js**
```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        use: [
          'cache-loader',
          'babel-loader'
        ],
        include: path.resolve('src')
      }
    ]
  }
}
```

### `数据库集成`

**webpack.config.js**
```js
// 或者不同的数据库客户端 - memcached, mongodb, ...
const redis = require('redis');
const crypto = require('crypto');

// ...
// 连接到数据库...
// ...

const BUILD_CACHE_TIMEOUT = 24 * 3600; // 1 day

function digest(str) {
  return crypto.createHash('md5').update(str).digest('hex');
}

// 生成缓存key
function cacheKey(options, request) {
  return `build:cache:${digest(request)}`;
}


// 从数据库读取数据并解析它们
function read(key, callback) {
  client.get(key, (err, result) => {
    if (err) {
      return callback(err);
    }

    if (!result) {
      return callback(new Error(`Key ${key} not found`));
    }

    try {
      let data = JSON.parse(result);
      callback(null, data);
    } catch (e) {
      callback(e);
    }
  });
}


// 写入数据到数据库对应的cacheKey
function write(key, data, callback) {
  client.set(key, JSON.stringify(data), 'EX', BUILD_CACHE_TIMEOUT, callback);
}

module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        use: [
          {
            loader: 'cache-loader',
            options: {
              cacheKey,
              read,
              write,
            }
          },
          'babel-loader'
        ],
        include: path.resolve('src')
      }
    ]
  }
}
```

<h2 align="center">维护者</h2>

<table>
  <tbody>
    <tr>
      <td align="center">
        <a href="https://github.com/sokra">
          <img width="150" height="150" src="https://github.com/sokra.png?size=150">
          </br>
          Tobias Koppers
        </a>
      </td>
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


[npm]: https://img.shields.io/npm/v/cache-loader.svg
[npm-url]: https://npmjs.com/package/cache-loader

[node]: https://img.shields.io/node/v/cache-loader.svg
[node-url]: https://nodejs.org

[deps]: https://david-dm.org/webpack-contrib/cache-loader.svg
[deps-url]: https://david-dm.org/webpack-contrib/cache-loader

[chat]: https://img.shields.io/badge/gitter-webpack%2Fwebpack-brightgreen.svg
[chat-url]: https://gitter.im/webpack/webpack

[test]: http://img.shields.io/travis/webpack-contrib/cache-loader.svg
[test-url]: https://travis-ci.org/webpack-contrib/cache-loader

[cover]: https://codecov.io/gh/webpack-contrib/cache-loader/branch/master/graph/badge.svg
[cover-url]: https://codecov.io/gh/webpack-contrib/cache-loader
