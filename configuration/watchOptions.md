# WatchOptions（观察选项）

用于定制watch模式。

## `watchOptions`

类型：object

一组用来定制 Watch 模式的选项：

```js
watchOptions: {
  aggregateTimeout: 300,
  poll: 1000
}
```

### `watchOptions.aggregateTimeout`

类型：number

默认值：300

单位：毫秒

当第一个文件更改，会在重新构建前增加延迟。这个选项允许 webpack 将这段时间内进行的任何其他更改都聚合到一次重新构建里。

```js
aggregateTimeout: 300 // 默认值
```

### `watchOptions.ignored`

对于某些系统，监听大量文件系统会导致大量的 CPU 或内存占用。这个选项可以排除一些巨大的文件夹，例如`node_modules`：

```js
ignored: /node_modules/
```

也可以使用[anymatch](https://github.com/es128/anymatch)模式：

```js
ignored: "files/**/*.js"
```

### `watchOptions.poll`

类型：boolean \| number

通过传递`true`开启[polling](http://whatis.techtarget.com/definition/polling)，或者指定毫秒为单位进行轮询。

```js
poll: 1000 // 每秒检查一次变动
```

> **\[info\]** 注：
>
> 如果监听没生效，试试这个选项吧。Watch 在 NFS 和 VirtualBox 机器上不适用。





