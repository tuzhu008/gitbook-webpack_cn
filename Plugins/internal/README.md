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
| `I18nWebpackPlugin` | Add i18n support to your bundles |  |
| `IgnorePlugin` | Exclude certain modules from bundles |  |
| `LimitChunkCountPlugin` | Set min/max limits for chunking to better control chunking |  |
| `LoaderOptionsPlugin` | Used for migrating from webpack 1 to 2 |  |
| `MinChunkSizePlugin` | Keep chunk size above the specified limit |  |
| `NoEmitOnErrorsPlugin` | Skip the emitting phase when there are compilation errors |  |
| `NormalModuleReplacementPlugin` | Replace resource\(s\) that matches a regexp |  |
| `NpmInstallWebpackPlugin` | Auto-install missing dependencies during development |  |
| `ProvidePlugin` | Use modules without having to use import/require |  |
| `SourceMapDevToolPlugin` | Enables a more fine grained control of source maps |  |
| `UglifyjsWebpackPlugin` | Enables control of the version of UglifyJS in your project |  |
| `ZopfliWebpackPlugin` | Prepare compressed versions of assets with node-zopfli |  |



