[![npm][npm]][npm-url]
[![deps][deps]][deps-url]
[![test][test]][test-url]
[![coverage][cover]][cover-url]
[![chat][chat]][chat-url]

<div align="center">
  <a href="https://webpack.js.org/">
    <img width="200" height="200" vspace="" hspace="25" src="https://webpack.js.org/assets/icon-square-big.svg">
  </a>
  <h1>thread-loader</h1>
  <p>在一个 worker 池中运行下面的loader。</p>
  <a href="https://github.com/webpack-contrib/thread-loader"><img src="https://img.shields.io/badge/Github-%E6%9F%A5%E7%9C%8B%E6%9B%B4%E5%A4%9A-brightgreen.svg"></a>
</div>

<h2 align="center">安装</h2>

```bash
npm install --save-dev thread-loader
```

<h2 align="center">用法</h2>

把此 loader 放置在其他 loader 之前， 放置在此 loader 之后的 loader 就会在一个单独的 worker 池(worker pool)中运行

在worker 池(worker pool)中运行的 loader 是受到限制的。例如：

* 这些 loader 不能产生新的文件。
* 这些 loader 不能使用自定义的 loader API（也就是说，通过插件）。
* 这些 loader 无法获取 webpack 的选项设置。

每个 worker 都是一个单独的有 600ms 开销的 node.js 进程。此外，进程间通信也有开销。

请仅在昂贵的操作上使用此 loader。

<h2 align="center">示例</h2>

**webpack.config.js**

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        include: path.resolve("src"),
        use: [
          "thread-loader",
          "expensive-loader"
        ]
      }
    ]
  }
}
```

**with options**

```js
use: [
  {
    loader: "thread-loader",
    // 有同样配置的 loader 会共享一个 worker 池(worker pool)
    options: {
      // 产生的 worker 的数量，默认是 cpu 的核心数
      workers: 2,

      // 一个 worker 进程中并行执行工作的数量
      // 默认为 20
      workerParallelJobs: 50,

      // 额外的 node.js 参数
      workerNodeArgs: ['--max-old-space-size', '1024'],

      // 闲置时定时删除 worker 进程
      // 默认为 500 (ms)
      // 可以设置为无穷大， 这样在观察模式(--watch)下可以保持 worker 持续存在
      poolTimeout: 2000,

      // 池(pool)分配给 worker 的工作数量
      // 数量 200
      // 降低这个数值会降低总体的效率，但是会提升工作分布更均一
      poolParallelJobs: 50,

      // 池(pool)的名称
      // 可以修改名称来创建其余选项都一样的池(pool)
      name: "my-pool"
    }
  },
  "expensive-loader"
]
```

**预热**

可以通过预热 worker 池(worker pool)来防止启动 worker 时的高延时。

这会启动池(pool)内最大数量的 worker 并把指定的模块载入 node.js 的模块缓存中。

``` js
const threadLoader = require('thread-loader');

threadLoader.warmup({
  // pool 选项, 就像传递loader选项一样
  // 必须匹配loader选项以引导正确的池
}, [
  // 模块加载
  // 可以是任何模块, 例如
  'babel-loader',
  'babel-preset-es2015',
  'sass-loader',
]);
```


<h2 align="center">维护者</h2>

<table>
  <tbody>
    <tr>
      <td align="center">
        <a href="https://github.com/sokra">
          <img width="150" height="150" src="https://github.com/sokra.png?size=150">
          </br>
          sokra
        </a>
      </td>
    </tr>
  <tbody>
</table>


[npm]: https://img.shields.io/npm/v/thread-loader.svg
[npm-url]: https://npmjs.com/package/thread-loader

[deps]: https://david-dm.org/webpack-contrib/thread-loader.svg
[deps-url]: https://david-dm.org/webpack-contrib/thread-loader

[chat]: https://img.shields.io/badge/gitter-webpack%2Fwebpack-brightgreen.svg
[chat-url]: https://gitter.im/webpack/webpack

[test]: http://img.shields.io/travis/webpack-contrib/thread-loader.svg
[test-url]: https://travis-ci.org/webpack-contrib/thread-loader

[cover]: https://codecov.io/gh/webpack-contrib/thread-loader/branch/master/graph/badge.svg
[cover-url]: https://codecov.io/gh/webpack-contrib/thread-loader
