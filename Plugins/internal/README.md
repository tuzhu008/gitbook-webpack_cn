# 内部插件

webpack本身内置了很多插件。

| 名称 | 描述 | 适用环境 |
| :--- | :--- | :---: |
| [`AggressiveSplittingPlugin`](//Plugins/internal/AggressiveSplittingPlugin.md) | 将原始chunk分割成更小的chunk |  |
| [`BannerPlugin`](/Plugins/internal/BannerPlugin.md) | 在每个生成的 chunk 顶部添加 banner |  |
| [`CommonsChunkPlugin`](/Plugins/internal/CommonsChunkPlugin.md) | 提取 chunks 之间共享的通用模块 |  |
| [`NamedModulesPlugin`](/Plugins/internal/NamedModulesPlugin.md) | HMR时显示模块的相对路径 | 生产 |
| [`HashedModuleIdsPlugin`](/Plugins/internal/HashedModuleIdsPlugin.md) | 根据模块的相对路径生成一个四位数的hash作为模块id | 生产 |
| [`ContextReplacementPlugin`](/Plugins/internal/ContextReplacementPlugin.md) | 重写 `require` 表达式的推断上下文 |  |
| [`DefinePlugin`](/Plugins/internal/DefinePlugin.md) | 允许在编译时配置全局常量 |  |
| [`DllPlugin`](/Plugins/internal/DllPlugin.md) | 分割bundle来大幅改善构建时间，分割bundle |  |
| [`EnvironmentPlugin`](/Plugins/internal/EnvironmentPlugin.md) | 内部使用`DefinePlugin` 来设置 `process.env` 键的快捷方式 |  |
| [`HotModuleReplacementPlugin`](/Plugins/internal/HotModuleReplacementPlugin.md) | 开启模块热替换 |  |
| [`IgnorePlugin`](/Plugins/internal/IgnorePlugin.md) | Exclude certain modules from bundles |  |
| [`LimitChunkCountPlugin`](/Plugins/internal/LimitChunkCountPlugin.md) | 限制chunk的数量和每个chunk的体积 |  |
| [`MinChunkSizePlugin`](/Plugins/internal/MinChunkSizePlugin.md) | 设置chunk的最小体积（字节） |  |
| [`NoEmitOnErrorsPlugin`](/Plugins/internal/NoEmitOnErrorsPlugin.md) | 在输出阶段时，遇到编译错误跳过 |  |
| [`NormalModuleReplacementPlugin`](/Plugins/internal/NormalModuleReplacementPlugin.md) | 替换与正则表达式匹配的资源 |  |
| [`ProvidePlugin`](/Plugins/internal/ProvidePlugin.md) | 不必通过 import/require 使用模块 |  |
| `SourceMapDevToolPlugin` | Enables a more fine grained control of source maps |  |
| `UglifyjsWebpackPlugin` | Enables control of the version of UglifyJS in your project |  |
| `ZopfliWebpackPlugin` | Prepare compressed versions of assets with node-zopfli |  |
| [PrefetchPlugin](/Plugins/internal/PrefetchPlugin.md) |  |  |



