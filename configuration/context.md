# context

context取值：`string`

```js
context: path.resolve(__dirname, "app")
```

context表示webpack的基础目录，它是一个绝对路径。用于从配置中解析入口起点（entry pointer）和 loader。

