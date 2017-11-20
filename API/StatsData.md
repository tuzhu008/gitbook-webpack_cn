# Stats Data \(统计数据）

通过 webpack 编译源文件时，用户可以生成包含有关于模块的统计数据的 JSON 文件。这些统计数据不仅可以帮助开发者来分析应用的依赖图表，还可以优化编译的速度。这个 JSON 文件可以通过以下的命令来生成:

```bash
webpack --profile --json > compilation-stats.json
```

这个标识是告诉 webpack`compilation-stats.json`要包含依赖的图表以及各种其他的编译信息。一般来说，也会把`--profile`一起加入，这样每一个包含自身编译数据的[`module` 对象](https://doc.webpack-china.org/api/stats/#modules-object)都会添加`profile`。

## 结构（Structure）

最外层的输出 JSON 文件比较容易理解，但是其中还是有一小部分嵌套的数据不是那么容易理解。不过放心，这其中的每一部分都在后面有更详细的解释，并且注释中还附带有超链接可以直接跳入相应的章节。

```json
{
  "version": "1.4.13", // 用来编译的 webpack 的版本
  "hash": "11593e3b3ac85436984a", // 编译使用的 hash
  "time": 2469, // 编译耗时 (ms)
  "filteredModules": 0, // 当 `exclude`传入`toJson` 函数时，统计被无视的模块的数量
  "assetsByChunkName": {
    // 用作映射的 chunk 的名称
    "main": "web.js?h=11593e3b3ac85436984a",
    "named-chunk": "named-chunk.web.js",
    "other-chunk": [
      "other-chunk.js",
      "other-chunk.css"
    ]
  },
  "assets": [
    // asset 对象 (asset objects) 的数组
  ],
  "chunks": [
    // chunk 对象 (chunk objects) 的数组
  ],
  "modules": [
    // 模块对象 (module objects) 的数组
  ],
  "errors": [
    // 错误字符串 (error string) 的数组
  ],
  "warnings": [
    // 警告字符串 (warning string) 的数组
  ]
}

```

### Asset对象 \(Asset Objects\)

每一个`asset`对象都表示一个编译出的`output`文件。`asset`都会有一个共同的结构：

```json
{
  "chunkNames": [], // 这个 asset 包含的 chunk
  "chunks": [ 10, 6 ], // 这个 asset 包含的 chunk 的 id
  "emitted": true, // 表示这个 asset 是否会让它输出到 output 目录
  "name": "10.web.js", // 输出的文件名
  "size": 1058 // 文件的大小
}
```

### Chunk对象 \(Chunk Objects\)

每一个`chunks`表示一组称为 [chunk](https://doc.webpack-china.org/glossary#c) 的模块。每一个对象都满足以下的结构。



