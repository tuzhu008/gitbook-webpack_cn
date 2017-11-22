# SourceMapDevToolPlugin

该插件支持对源映射生成进行更细粒度的控制。它是[devtools](//configuration/devtools.md)配置选项的另一种选择。

```js
new webpack.SourceMapDevToolPlugin(options)
```

## options

类型：object

支持以下选项：

* `test`\(`string|regex|array`\)：包含基于其扩展的模块的源映射\(默认为 `.js`and`.css`\).
* `include`\(`string|regex|array`\)：包含与给定值匹配的模块路径的源映射。
* `exclude`\(`string|regex|array`\)：从源映射生成中排除匹配给定值的模块。
* `filename`\(`string`\)：定义SourceMap的输出文件名 \(如果没有提供任何值，就会被内联\).
* `append`\(`string`\)： 将给定的值添加到原始资源 。Usually the`#sourceMappingURL`comment.`[url]`被替换为源映射文件的URL。`false`禁用的添加。
* `moduleFilenameTemplate`\(`string`\)：查看[`output.devtoolModuleFilenameTemplate`](/configuration/output.md#https://doc.webpack-china.org/configuration/output/#output-devtoolmodulefilenametemplate).
* `fallbackModuleFilenameTemplate`\(`string`\)：参见上面的链接。
* `module`\(`boolean`\)：指示loader是否应该生成源映射\(默认为`true`\).
* `columns`\(`boolean`\)：表示是否应该使用列映射 \(默认为`true`\).
* `lineToLine`\(`object`\): 通过为匹配到的模块使用行到行（line to line）源映射来简化和加速源映射。

`lineToLine`对象允许上面描述的相同的`test`、`include`、以及`exclude`选项。

> **\[info\]** 注：
>
> 将`module`和/或 `columns`设置为`false`将生成更少的精确的源映射，但也会显著提高编译性能。

## 用法：排除 Vendor 的 Map 文件

以下代码将排除`vendor.js`bundle 中任何模块的 source map：

```js
new webpack.SourceMapDevToolPlugin({
  filename: '[name].js.map',
  exclude: ['vendor.js']
})
```

## 进一步阅读

* [Building Source Maps](https://survivejs.com/webpack/building/source-maps/#-sourcemapdevtoolplugin-and-evalsourcemapdevtoolplugin-) 



