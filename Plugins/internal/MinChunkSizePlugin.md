# MinChunkSizePlugin

通过合并小于`minChunkSize`大小的 chunk，将 chunk 体积保持在指定大小限制以上。

```js
new webpack.optimize.MinChunkSizePlugin({
  minChunkSize: 10000 // 字符的最小数量
})
```



