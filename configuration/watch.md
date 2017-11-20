# Watch（观察）

webpack 可以监听文件变化，当它们修改后会重新编译。这个页面介绍了如何启用这个功能，以及当 watch 无法正常运行的时候你可以做的一些调整。

## `watch`

类型：boolean

默认值：false

启用 Watch 模式。这意味着在初始构建之后，webpack 将继续监听任何已解析文件的更改。Watch 模式默认关闭。

```js
watch: false
```

> **\[info\]** 注：
>
> webpack-dev-server 和 webpack-dev-middleware 里 Watch 模式默认开启。

更多关于watch的配置选项，请参见[watchOptions](/configuration/watchOptions.md)



