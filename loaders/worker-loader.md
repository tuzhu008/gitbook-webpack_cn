[![npm][npm]][npm-url]
[![node][node]][node-url]
[![deps][deps]][deps-url]
[![test][test]][test-url]
[![coverage][cover]][cover-url]
[![chat][chat]][chat-url]

<div align="center">
  <a href="https://github.com/webpack/webpack">
    <img width="200" height="200" src="https://webpack.js.org/assets/icon-square-big.svg">
  </a>
  <h1>Worker Loader</h1>
  <p>这个 loader 将脚本注册为 <a href="https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API">Web Worker</a><p>
  <a href="https://github.com/webpack-contrib/worker-loader"><img src="https://img.shields.io/badge/Github-%E6%9F%A5%E7%9C%8B%E6%9B%B4%E5%A4%9A-brightgreen.svg"></a>
</div>


<h2 align="center">安装</h2>

```bash
npm i -D worker-loader
```

<h2 align="center"><a href="https://webpack.js.org/concepts/loaders">用法</a></h2>

### `内联`

**App.js**
```js
import Worker from 'worker-loader!./Worker.js';
```

### `配置`

**webpack.config.js**
```js
{
  module: {
    rules: [
      {
        test: /\.worker\.js$/,
        use: { loader: 'worker-loader' }
      }
    ]
  }
}
```

**App.js**
```js
import Worker from './file.worker.js';

const worker = new Worker();

worker.postMessage({ a: 1 });
worker.onmessage = function (event) {};

worker.addEventListener("message", function (event) {});
```

<h2 align="center">选项</h2>

|名称|类型|默认值|描述|
|:--:|:--:|:-----:|:----------|
|[**`name`**](#name)|`{String}`|`[hash].worker.js`| 为输出脚本设置一个自定义的名字 |
|[**`inline`**](#inline)|`{Boolean}`|`false`| 将 worker 内联为 BLOB |
|[**`fallback`**](#fallback)|`{Boolean}`|`false`| 对非 worker 环境的支持环境require 一个备用 |
|[**`publicPath`**](#publicPath)|`{String}`|`null`| 覆盖来自worker 脚本被下载的路径 |

### `name`

使用 `name` 参数为输出脚本设置一个自定义的名字。这个参数可以包含`[hash]`字符串，它将被『用于缓存目的』的『内容相关哈希』取代

*webpack.config.js**
```js
{
  loader: 'worker-loader',
  options: { name: 'WorkerName.[hash].js' }
}
```

### `inline`

可以使用 `inline` 参数将 worker 内联为 BLOB

**webpack.config.js**
```js
{
  loader: 'worker-loader',
  options: { inline: true }
}
```

> **[info]** 
> 
> 内联模式还将为不支持内联 worker 的浏览器创建 chunk ，为了禁止该行为，只需将 `fallback` 参数设置为 `false`

**webpack.config.js**
```js
{
  loader: 'worker-loader'
  options: { inline: true, fallback: false }
}
```

### `fallback`

为非 worker 环境支持 rerquire 一个回退方案。

**webpack.config.js**
```js
{
  loader: 'worker-loader'
  options: { fallback: false }
}
```

### `publicPath`


覆盖来自 worker 脚本被下载的路径。如果未指定，则使用其他 webpack 资产的相同公共路径使用其他webpack资产的相同公共路径

**webpack.config.js**
```js
{
  loader: 'worker-loader'
  options: { publicPath: '/scripts/workers/' }
}
```

<h2 align="center">示例</h2>

文件可以像其他文件那样导入依赖。

**Worker.js**
```js
const _ = require('lodash')

const obj = { foo: 'foo' }

_.has(obj, 'foo')

// 将数据发送到父线程
self.postMessage({ foo: 'foo' })

// 对来自父线程的消息进行响应
self.addEventListener('message', (event) => console.log(event))  
```

### `与 ES2015 模块集成`

> **[info]**
>
> 如果配置了 [`babel-loader`](//loaders/babel-loader.md)，甚至可以使用 ES2015 模块。

**Worker.js**
```js
import _ from 'lodash'

const obj = { foo: 'foo' }

_.has(obj, 'foo')

// 将数据发送到父线程
self.postMessage({ foo: 'foo' })

// 对来自父线程的消息进行响应
self.addEventListener('message', (event) => console.log(event))
```

### `与 TypeScript 集成`

为了与 TypeScript 集成，需要为 worker 的导出定义一个自定义的模块

**typings/custom.d.ts**
```ts
declare module "worker-loader!*" {
  class WebpackWorker extends Worker {
    constructor();
  }

  export = WebpackWorker;
}
```

**Worker.ts**
```ts
const ctx: Worker = self as any;

// Post data to parent thread
ctx.postMessage({ foo: "foo" });

// Respond to message from parent thread
ctx.addEventListener("message", (event) => console.log(event));
```

**App.ts**
```ts
import Worker = require("worker-loader!./Worker");

const worker = new Worker();

worker.postMessage({ a: 1 });
worker.onmessage = (event) => {};

worker.addEventListener("message", (event) => {});
```

### `跨域规则（Cross-Origin Policy）`

[`WebWorkers`](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API) 受到[同源规则（same-origin policy）](https://en.wikipedia.org/wiki/Same-origin_policy)的限制，因此，如果您的 webpack 资源与您的应用程序来自不同的域，那么它们的下载可能会被浏览器阻塞。如果您在一个CDN域下托管您的资源，这种情况通常会发生。甚至从 `webpack-dev-server` 上下载都会被阻塞。有两个解决方法：

首先，您可以将 worker 内联为 blob，而不是通过[inline](#inline) 参数将其作为外部脚本下载

**App.js**
```js
import Worker from './file.worker.js';
```

**webpack.config.js**
```js
{
  loader: 'worker-loader'
  options: { inline: true }
}
```

其次，您可以通过[`publicPath`](#publicpath)选项覆盖您的 worker 脚本的基本下载URL

**App.js**
```js
// 这将导致 worker 从 `/workers/file.worker.js`
 中下载
import Worker from './file.worker.js';
```

**webpack.config.js**
```js
{
  loader: 'worker-loader'
  options: { publicPath: '/workers/' }
}
```

<h2 align="center">维护者</h2>

<table>
  <tbody>
    <tr>
      <td align="center">
        <a href="https://github.com/TrySound">
          <img width="150" height="150" src="https://avatars3.githubusercontent.com/u/5635476?v=3&s=150">
        </a>
        <br />
        <a href="https://github.com/TrySound">Bogdan Chadkin</a>
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


[npm]: https://img.shields.io/npm/v/worker-loader.svg
[npm-url]: https://npmjs.com/package/worker-loader

[node]: https://img.shields.io/node/v/cache-loader.svg
[node-url]: https://nodejs.org

[deps]: https://david-dm.org/webpack-contrib/worker-loader.svg
[deps-url]: https://david-dm.org/webpack-contrib/worker-loader

[test]: http://img.shields.io/travis/webpack-contrib/worker-loader.svg
[test-url]: https://travis-ci.org/webpack-contrib/worker-loader

[cover]: https://codecov.io/gh/webpack-contrib/cache-loader/branch/master/graph/badge.svg
[cover-url]: https://codecov.io/gh/webpack-contrib/cache-loader

[chat]: https://img.shields.io/badge/gitter-webpack%2Fwebpack-brightgreen.svg
[chat-url]: https://gitter.im/webpack/webpack
