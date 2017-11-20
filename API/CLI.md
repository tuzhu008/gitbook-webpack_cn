# 命令行接口（CLI）

为了更合适且方便地使用配置，可以在`webpack.config.js`中对 webpack 进行配置。CLI 中传入的任何参数会在配置文件中映射为对应的参数。

## 使用配置文件

```bash
webpack [--config webpack.config.js]
```

配置文件中的相关选项，请参阅[配置](//configuration/README.md)。

## 不使用配置文件

```bash
webpack <entry> [<entry>] <output>
```

**`<entry>`**

一个文件名或一个文件名数组，作为构建项目的入口起点。您可以传递多个入口（每个入口在启动时加载）。如果传递一个形式为`<name> = <request>`的键值对，则可以创建一个额外的入口起点。它将被映射到配置选项的`entry`属性。

**`<output>`**

要保存的 bundle 文件的路径和文件名。它将映射到配置选项`output.path`和`output.filename`。

**示例:**

假设你的项目结构像下面这样：

```
.
├── dist
├── index.html
└── src
    ├── index.js
    ├── index2.js
    └── others.js
```

终端执行：

```bash
webpack src/index.js dist/bundle.js
```

打包源码，入口为`index.js`，并且输出文件的路径为`dist`文件夹，文件名为`bundle.js`

终端输出：

```
    | Asset     | Size    | Chunks      | Chunk Names |
    |-----------|---------|-------------|-------------|
    | bundle.js | 1.54 kB | 0 [emitted] | index       |
    [0] ./src/index.js 51 bytes {0} [built]
    [1] ./src/others.js 29 bytes {0} [built]
```

多入口：

```bash
webpack index=./src/index.js entry2=./src/index2.js dist/bundle.js
```

```
    | Asset     | Size    | Chunks        | Chunk Names   |
    |-----------|---------|---------------|---------------|
    | bundle.js | 1.55 kB | 0,1 [emitted] | index, entry2 |
    [0] ./src/index.js 51 bytes {0} [built]
    [0] ./src/index2.js 54 bytes {1} [built]
    [1] ./src/others.js 29 bytes {0} {1} [built]
```

## 常用选项

**列出命令行所有可用的配置选项：**

```bash
webpack --help
webpack -h
```

**使用配置文件进行构建：**

指定其它的[配置](//configuration/README.md)文件。配置文件默认为`webpack.config.js`，如果你想使用其它配置文件，可以加入这个参数。

```bash
webpack --config example.config.js
```

**以 JSON 格式输出 webpack 的运行结果：**

```bash
webpack --json
webpack --json > stats.json
```

在其他每个情况下，webpack 会打印一组统计信息，用于显示 bundle, chunk 和用时等详细信息。使用此选项，输出可以是 JSON 对象。此输出文件\(response\)可被 webpack 的[分析工具](https://webpack.github.com/analyse)，或 chrisbateman 的[webpack 可视化工具](https://chrisbateman.github.io/webpack-visualizer/)，或 th0r 的[webpack bundle 分析工具](https://github.com/th0r/webpack-bundle-analyzer)接收后进行分析。分析工具将接收 JSON 并以图形形式提供构建的所有细节。



## 环境选项

当 webpack 配置对象[导出为一个函数](https://doc.webpack-china.org/configuration/configuration-types#exporting-a-function)时，可以向这个函数传入一个"环境对象\(environment\)"。

```bash
webpack --env.production    # sets env.production == true
webpack --env.platform=web  # sets env.platform == "web"
```

--env参数接受各种语法:

Invocation                               | Resulting environment
---------------------------------------- | ---------------------------
`webpack --env prod`                     | `"prod"`
`webpack --env.prod`                     | `{ prod: true }`
`webpack --env.prod=1`                   | `{ prod: 1 }`
`webpack --env.prod=foo`                 | `{ prod: "foo" }`
`webpack --env.prod --env.min`           | `{ prod: true, min: true }`
`webpack --env.prod --env min`           | `[{ prod: true }, "min"]`
`webpack --env.prod=foo --env.prod=bar`  | `{prod: [ "foo", "bar" ]}`



参数 | 说明 | 输入类型 | 默认值
------------------------- | ------------------------------------------- | ---------- | ------------------
`--output-chunk-filename` | 输出的附带 chunk 的文件名   | string     | 含有 [id] 的文件名，而不是 [name] 或者 [id] 作为前缀
`--output-filename`       | 打包文件的文件名           | string     | [name].js
`--output-jsonp-function` | 加载 chunk 时使用的 JSONP 函数名 | string | webpackJsonp
`--output-library`        | 以库的形式导出入口文件 | string |
`--output-library-target` | 以库的形式导出入口文件时，输出的类型 | string | var
`--output-path`           | 输出的路径（在公共路径的基础上）      | string     | 当前目录
`--output-pathinfo`       | 加入一些依赖信息的注解 | boolean | false
`--output-public-path`    | The 输出文件时使用的公共路径              | string     | /
`--output-source-map-filename` | 生成的 SourceMap 的文件名  | string     | [name].map or [outputFilename].map

参数 | 说明 | 输入类型 | 默认值
------------ | ------------------------------------------------ | ---------- | -------------
`--debug`    | 把 loader 设置为 debug 模式 | boolean    | false
`--devtool`  | 为打包好的资源定义 [source map 的类型] | string | -
`--progress` | 打印出编译进度的百分比值 | boolean    | false


参数 | 说明
------------------------- | ----------------------
`--watch`, `-w`           | 观察文件系统的变化
`--save`, `-s`            | 在保存的时候重新编译，无论文件是否变化
`--watch-aggregate-timeout` | 指定一个毫秒数，在这个时间内，文件若发送了多次变化，会被合并
`--watch-poll`            | 轮询观察文件变化的时间间隔（同时会打开轮询机制）
`--watch-stdin`, `--stdin` | 当 stdin 关闭时，退出进程


参数 | 说明 | 使用的插件
--------------------------- | -------------------------------------------------------|----------------------
`--optimize-max-chunks`     | 限制 chunk 的数量 | [LimitChunkCountPlugin](/plugins/limit-chunk-count-plugin)
`--optimize-min-chunk-size` | 限制 chunk 的最小体积               | [MinChunkSizePlugin](/plugins/min-chunk-size-plugin)
`--optimize-minimize`       | 压缩混淆 javascript，并且把 loader 设置为 minimizing | [UglifyJsPlugin](/plugins/uglifyjs-webpack-plugin/) & [LoaderOptionsPlugin](/plugins/loader-options-plugin/)



参数   | 说明                      | 示例
---------------------- | ------------------------------------------------------- | -------------
--resolve-alias        | 指定模块的别名 | --resolve-alias jquery-plugin=jquery.plugin
--resolve-extensions   | 指定需要被处理的文件的扩展名 | --resolve-extensions .es6 .js .ts
--resolve-loader-alias | Minimize javascript and switches loaders to minimizing  |

参数 | 说明 | Type
-------------------------------- | ------------------------------------------------------------------ | -------
`--color`, `--colors` | E开启/关闭控制台的颜色 [默认值：(supports-color)] | boolean
`--display`                      | 选择[显示预设](/configuration/stats)(verbose - 繁琐, detailed - 细节, normal - 正常, minimal - 最小, errors-only - 仅错误, none - 无; 从 webpack 3.0.0 开始) | string
`--display-cached` | 在输出中显示缓存的模块 | boolean
`--display-cached-assets` | 在输出中显示缓存的 assets | boolean
`--display-chunks` | 在输出中显示 chunks | boolean
`--display-depth` | 显示从入口起点到每个模块的距离 | boolean
`--display-entrypoints`   | 在输出中显示入口文件 | boolean
`--display-error-details` | 显示详细的错误信息 | boolean
`--display-exclude`       | 在输出中显示被排除的文件 | boolean
`--display-max-modules`          | 设置输出中可见模块的最大数量 | number
`--display-modules` | 在输出中显示所有模块，包括被排除的模块 | boolean
`--display-optimization-bailout` | 作用域提升回退触发器(Scope hoisting fallback trigger)（从 webpack 3.0.0 开始） | boolean
`--display-origins` | 在输出中显示最初的 chunk | boolean
`--display-provided-exports`     | 显示有关从模块导出的信息 | boolean
`--display-reasons` | 显示模块包含在输出中的原因 | boolean
`--display-used-exports` | 显示模块中被使用的接口（Tree Shaking） | boolean
`--hide-modules` | 隐藏关于模块的信息 | boolean
`--sort-assets-by` | 对 assets 列表以某种属性排序 | string
`--sort-chunks-by`               | 对 chunks 列表以某种属性排序 | string
`--sort-modules-by`              | 对模块列表以某种属性排序 | string
`--verbose`                      | 显示更多信息 | boolean



参数 | 说明 | 用法
----------------- | ---------------------------------------- | -----
`--bail` | 一旦发生错误，立即终止 |
`--cache` | 开启缓存 [watch 时会默认打开] | `--cache=false`
`--define` | 定义 bundle 中的任意自由变量，查看 [shimming](/guides/shimming) | `--define process.env.NODE_ENV='development'`
`--hot`           | 开启[模块热替换](/concepts/hot-module-replacement) | `--hot=true`
`--labeled-modules` | 开启模块标签 [使用 LabeledModulesPlugin] |
`--plugin`        | 加载某个[插件](/configuration/plugins/) |
`--prefetch`      | 预加载某个文件 | `--prefetch=./files.js`
`--provide`       | 在所有模块中将这些模块提供为自由变量，查看 [shimming](/guides/shimming) | `--provide jQuery=jquery`
`--records-input-path` | 记录文件的路径（读取） |
`--records-output-path` | 记录文件的路径（写入） |
`--records-path`  | 记录文件的路径 |
`--target`        | [目标](/configuration/target/)的执行环境 | `--target='node'`



简写 | 含义
---------|----------------------------
-d       | `--debug --devtool cheap-module-eval-source-map --output-pathinfo`
-p       | `--optimize-minimize --define process.env.NODE_ENV="production"`, see [building for production](/guides/production)


