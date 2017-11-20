# Stats（统计）

stats选项允许您精确控制哪些bundle信息被显示。如果你不想使用`quiet`或`noInfo`，这可能是一个很好的中间地带，因为你想要一些bundle信息，但不是全部。

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
  // fallback value for stats options when an option is not defined (has precedence over local webpack defaults)
  all: undefined,
  // Add asset Information
  assets: true,
  // Sort assets by a field
  // You can reverse the sort with `!field`.
  assetsSort: "field",
  // Add information about cached (not built) modules
  cached: true,
  // Show cached assets (setting this to `false` only shows emitted files)
  cachedAssets: true,
  // Add children information
  children: true,
  // Add chunk information (setting this to `false` allows for a less verbose output)
  chunks: true,
  // Add built modules information to chunk information
  chunkModules: true,
  // Add the origins of chunks and chunk merging info
  chunkOrigins: true,
  // Sort the chunks by a field
  // You can reverse the sort with `!field`. Default is `id`.
  chunksSort: "field",
  // Context directory for request shortening
  context: "../src/",
  // `webpack --colors` equivalent
  colors: true,
  // Display the distance from the entry point for each module
  depth: false,
  // Display the entry points with the corresponding bundles
  entrypoints: false,
  // Add --env information
  env: false,
  // Add errors
  errors: true,
  // Add details to errors (like resolving log)
  errorDetails: true,
  // Exclude assets from being displayed in stats
  // This can be done with a String, a RegExp, a Function getting the assets name
  // and returning a boolean or an Array of the above.
  excludeAssets: "filter" | /filter/ | (assetName) => ... return true|false |
    ["filter"] | [/filter/] | [(assetName) => ... return true|false],
  // Exclude modules from being displayed in stats
  // This can be done with a String, a RegExp, a Function getting the modules source
  // and returning a boolean or an Array of the above.
  excludeModules: "filter" | /filter/ | (moduleSource) => ... return true|false |
    ["filter"] | [/filter/] | [(moduleSource) => ... return true|false],
  // See excludeModules
  exclude: "filter" | /filter/ | (moduleSource) => ... return true|false |
    ["filter"] | [/filter/] | [(moduleSource) => ... return true|false],
  // Add the hash of the compilation
  hash: true,
  // Set the maximum number of modules to be shown
  maxModules: 15,
  // Add built modules information
  modules: true,
  // Sort the modules by a field
  // You can reverse the sort with `!field`. Default is `id`.
  modulesSort: "field",
  // Show dependencies and origin of warnings/errors (since webpack 2.5.0)
  moduleTrace: true,
  // Show performance hint when file size exceeds `performance.maxAssetSize`
  performance: true,
  // Show the exports of the modules
  providedExports: false,
  // Add public path information
  publicPath: true,
  // Add information about the reasons why modules are included
  reasons: true,
  // Add the source code of modules
  source: true,
  // Add timing information
  timings: true,
  // Show which exports of a module are used
  usedExports: false,
  // Add webpack version information
  version: true,
  // Add warnings
  warnings: true,
  // Filter warnings to be shown (since webpack 2.4.0),
  // can be a String, Regexp, a function getting the warning and returning a boolean
  // or an Array of a combination of the above. First match wins.
  warningsFilter: "filter" | /filter/ | ["filter", /filter/] | (warning) => ... return true|false
};
```



