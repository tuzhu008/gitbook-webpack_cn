# AggressiveSplittingPlugin

`AggressiveSplittingPlugin`可以将bundle分割成小块（chunk）。分割每一个块,直到达到`options`配置`maxSize`。它通过文件夹结构将模块聚合（groups）在一起。

它在webpack记录中记录分割点，并尝试以相同的方式重建分割。这确保在对应用程序进行更改之后，以前的分割点\(和块\)被重用，因为它们可能已经在客户机的缓存中了。因此，强烈建议使用记录。

只有比指定的`minSize`大的块才被存储在记录中。这样可以确保在应用程序增长时，块会进行填充，而不是为每次更改都创建太多的块。

如果模块改变，块就会失效。来自无效块的模块将返回到模块池中，从中创建新的块。

```js
new webpack.optimize.AggressiveSplittingPlugin(options) // 使用前要先引入webpack
```

## options

类型: object

```js
{
  minSize: 30000, // 字节，分割点。默认：30720
  maxSize: 50000, // 字节，每个文件最大字节。默认：51200
  chunkOverhead: 0, // 默认：0
  entryChunkMultiplicator: 1, // 默认：1
}
```

## 示例

[http2-aggressive-splitting](https://github.com/webpack/webpack/tree/master/examples/http2-aggressive-splitting)

