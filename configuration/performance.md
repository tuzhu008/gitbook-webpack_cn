# Performance（性能）

这些选项可以控制 webpack 如何通知「资源\(asset\)和入口起点超过指定文件限制」。 此功能受到[webpack 性能评估](https://github.com/webpack/webpack/issues/3216)的启发。

## `performance`

类型：object

配置如何展示性能提示。例如，如果一个资源超过 250kb，webpack 会对此输出一个警告来通知你。

### `performance.hints`

类型：false \| "error" \| "warning"

默认值： "warning"

打开/关闭提示。此外，当找到提示时，告诉 webpack 抛出一个错误或警告。此属性默认设置为`"warning"`。

给定一个创建后超过 250kb 的资源：

```js
performance: {
  hints: false
}
```



