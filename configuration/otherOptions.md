# OtherOptions（其他选项）

这些是webpack支持的其他配置选项。

> **\[warning\]** 注：
>
> 招聘: 这一页仍在进行中。如果您熟悉描述或示例不完整的任何选项，请创建一个问题并在[docs repo](https://github.com/webpack/webpack.js.org)中提交一个PR。

## `amd`

类型：object

Set the value of `require.amd` or `define.amd`:

设置`require.amd`或`define.amd`的值

```js
amd: {
  jQuery: true
}
```

为AMD编写的一些流行模块，尤其是jQuery版本1.7.0到1.9.1，如果loader表明它已经为一个页面上的多个版本提供了[special allowances](https://github.com/amdjs/amdjs-api/wiki/jQuery-and-AMD)，那么它只会作为一个AMD模块注册。

这些allowances可以限制注册到特定的版本，或者支持不同的沙盒和不同的定义模块。

这个选项允许您将模块的键设置为一个真（truethy）值。事实上，在webpack中，AMD的支持忽略了定义的名称。

## `bail`

类型：boolean

在第一个错误上输出失败，而不是容忍它。默认情况下，webpack将在终端中标红记录这些错误，以及使用HMR时的浏览器控制台打印，但仍将继续绑定。启用请设置:

```js
bail: true
```

This will force webpack to exit its bundling process.这将迫使webpack退出打包（bundling）流程。

## `cache`

类型：boolean \| object

缓存生成的webpack模块和chunk，以提高构建速度。在监视\(Watch\)模式下，默认启用高速缓存。要禁用缓存，只需通过:

```js
cache: false
```

如果`cache`是一个对象，webpack将使用这个对象进行缓存。保留对这个对象的引用将允许在编译器调用之间共享相同的缓存:

```js
let SharedCache = {};

export default {
  ...,
  cache: SharedCache
}
```

> **\[warning\]** 注：
>
> 不要在不同选项的调用之间共享缓存。

?&gt; Elaborate on the warning and example - calls with different configuration options?

## `loader`

类型：object

将自定义值暴露到loader上下文中。

?&gt; Add an example...

## `parallelism`

类型：number

限制并行处理模块的数量。可用于调优性能或获得更可靠的性能分析结果。

## `profile`

类型：boolean

捕获应用程序的“配置文件”，包括统计信息（statistics）和提示（hints），然后使用[分析工具](https://webpack.github.io/analyse/)进行分析。

> **\[info\]** 注：
>
> 使用[StatsPlugin](https://www.npmjs.com/package/stats-webpack-plugin)来对生成的配置文件有更多的控制。

-

> **\[info\]** 注：
>
> 结合 `parallelism: 1` 获得更好的结果

## `recordsPath`

使用这个选项来生成包含 webpack “记录”的 JSON 文件。这个文件记录了数次构建时的模块的特征。你可以用这个文件来比较各个构建之间模块的改变。只要简单的设置一下路径就可以生成这个 JSON 文件：

```js
recordsPath: path.join(__dirname, 'records.json')
```

当使用利用[代码分离\(code splittnig\)](https://doc.webpack-china.org/guides/code-splitting)的复杂配置的时候，记录会特别有用。这个 JSON 文件可以用来确保被分割的 bundle 文件的确根据你的需求被保存进入了[缓存\(caching\)](https://doc.webpack-china.org/guides/caching)。

> **\[info\]** 注：
>
> 尽管这个文件是由编译器生成的，但是您可能仍然希望在源代码控制中跟踪它，以保留它随时间变化的历史。

-

> **\[warning\]** 注：
>
> 设置`recordsPath`会同时把`recordsInputPath`和`recordsOutputPath`设置成相同的路径。通常来讲这也是符合逻辑的，除非你想改变记录文件的名称。可以查看下面的实例：

## `recordsInputPath`

指定要读取最后一组记录的文件。这可以用来重命名一个记录文件。看下面的例子。

## `recordsOutputPath`

设定记录要写入的位置。下文的例子描述了如何用这个选项和`recordsInptuPaht`来重命名一个记录文件：

```js
recordsInputPath: path.join(__dirname, 'records.json'),
recordsOutputPath: path.join(__dirname, 'newRecords.json')
```



