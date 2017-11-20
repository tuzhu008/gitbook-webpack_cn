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

