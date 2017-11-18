# 上下文（context）

context是入口文件所处的目录的绝对路径的字符串。

context取值：`string`

```js
context: path.resolve(__dirname, "app")
```

context表示webpack的基础目录，它是一个绝对路径。用于从配置中解析入口起点（entry pointer）和 loader。

默认使用当前目录，但是推荐在配置中传递一个值。这使得你的配置独立于 CWD\(current working directory - 当前执行路径\)。

