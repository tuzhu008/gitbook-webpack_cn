# Stats（统计）

stats选项允许您精确控制哪些bundle信息被显示。如果你不想使用`quiet`或`noInfo`，这可能是一个很好的中间地带，因为你想要一些bundle信息，但不是全部。

```js
module.exports = {
    stats: string | Object
}
```

> **\[info\]**注：
>
> 对于webpack-dev-server，该属性需要在`devServer`对象中。

-

> **\[warning\]** 注：
>
> 这个选项在使用Node.js API 时没有任何效果。

## `stats`

类型：object \| string

有一些预设可用作快捷方式。使用方法：

```js
stats: "errors-only"
```

| Preset（预设） | Alternative（替代） | Description （描述） |
| --- | --- | --- |
| `"errors-only"` | _none_ | 只有当错误发生时才输出 |
| `"minimal"` | _none_ | 只有当错误或新的编译发生时才输出 |
| `"none"` | `false` | 不输出 |
| `"normal"` | `true` | 标准输出 |
| `"detailed"` | _none_ | 详细的输出 \(从 webpack 3.0.0起\) |
| `"verbose"` | _none_ | 输出所有的 |

为了获得更细粒度的控制，可以指定您想要的信息。请注意，此对象中的所有选项都是可选的。

```js
stats: {
  //stats选项的回退值，当一个选项没有被定义时（优先于本地webpack默认值）
  all: undefined,
  // 添加资源信息
  assets: true,
  // 按一个字段对资源进行排序
  // 你可以用'！字段'来反转排序。
  assetsSort: "field",
  // 添加关于cached(不是构建缓存) 模块的信息
  cached: true,
  // 展示 cached 资源 (将此设置为`false`，只显示发出（emitted）的文件)
  cachedAssets: true,
  // 添加子（children）信息
  children: true,
  // 添加 chunk 信息 (将此设置为false，可以使输出更少)
  chunks: true,
  // 添加构建模块信息到chunk信息
  chunkModules: true,
  // 添加chunk的源和chunk合并信息
  chunkOrigins: true,
  // 使用字段对chunk进行排序
  // 你可以使用`!field`来反转排序。默认为：`id`
  chunksSort: "field",
  // request shortening的上下文目录
  context: "../src/",
  // 与`webpack --colors` 等效
  colors: true,
  // 显示每个模块的入口点的距离
  depth: false,
  // 用相应的bundle显示入口点
  entrypoints: false,
  // 添加 --env 信息
  env: false,
  // 添加 errors
  errors: true,
  // 为 errors 增加细节 (像 resolving log)
  errorxuaDetails: true,
  // 排除在stats中显示的资产
  // 可以使用String、RegExp和获取资产名并返回一个布尔值或以上的这些的数组的函数来完成
  excludeAssets: "filter" | /filter/ | (assetName) => ... return true|false |
    ["filter"] | [/filter/] | [(assetName) => ... return true|false],
  // 排除在stats中显示的模块
  // 可以使用String、RegExp和获取资产名并返回一个布尔值或以上的这些的数组的函数来完成
  excludeModules: "filter" | /filter/ | (moduleSource) => ... return true|false |
    ["filter"] | [/filter/] | [(moduleSource) => ... return true|false],
  // 看到 excludeModules
  exclude: "filter" | /filter/ | (moduleSource) => ... return true|false |
    ["filter"] | [/filter/] | [(moduleSource) => ... return true|false],
  // 添加编译的hash
  hash: true,
  // 设置要显示的模块的最大数量
  maxModules: 15,
  // Add built modules information添加构建模块信息
  modules: true,
  // 使用字段对模块进行排序
  // 你可以使用 `!field`翻转排序结果，默认为 `id`.
  modulesSort: "field",
  // 显示相关性和错误的来源(自webpack 2.5.0)
  moduleTrace: true,
  // 当文件大小超过`performance.maxAssetSize`，显示性能提示。
  performance: true,
  // 显示模块的导出
  providedExports: false,
  // 添加公共路径信息
  publicPath: true,
  // 添加关于模块被包含的原因的信息
  reasons: true,
  // 添加模块的源代码
  source: true,
  // 添加时间信息
  timings: true,
  // 显示被使用的模块的导出
  usedExports: false,
  // 添加webpack版本信息
  version: true,
  // 添加警告
  warnings: true,
  // 过滤想要显示的警告(从 webpack 2.4.0),
  // 可以是 String, Regexp, 一个获取警告并返回一个布尔值或者以上组合的数组。都一个匹配的（First match wins.）。
  warningsFilter: "filter" | /filter/ | ["filter", /filter/] | (warning) => ... return true|false
};
```



