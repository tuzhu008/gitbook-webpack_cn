# DevServer（开发服务）

webpack-dev-server 能够用于快速开发应用程序。

此页面描述影响 webpack-dev-server\(简写为：dev-server\) 行为的选项。

> **\[info\] 注：**
>
> 与[webpack-dev-middleware](https://github.com/webpack/webpack-dev-middleware)兼容的选项旁边有 🔑

```js
module.exports = {
    devServer: {
        ...
    }
}
```

## devServer

类型：object

通过来自[webpack-dev-server](https://github.com/webpack/webpack-dev-server)的这些选项，能够用多种方式改变其行为。这里有一个简单的例子，所有来自`dist/`目录的文件都做 gzip 压缩，并提供为服务：

```js
devServer: {
  contentBase: path.join(__dirname, "dist"),
  compress: true,
  port: 9000
}
```

当服务器启动时，在解析模块列表之前会有一条消息：

```bash
http://localhost:9000/
webpack output is served from /build/
Content not from webpack is served from /path/to/dist/
```

这将给出一些关于服务器的位置，以及服务本身的背景信息。

如果你通过 Node.js API 来使用 dev-server，`devServer`中的选项将被忽略。将选项作为第二个参数传入：`new WebpackDevServer(compiler, {...})`。关于如何通过 Node.js API 使用 webpack-dev-server 的示例，请[查看此处](https://github.com/webpack/webpack-dev-server/blob/master/examples/node-api-simple/server.js)。

> **\[warning\]** 注：
>
> 在导出[多个配置](https://doc.webpack-china.org/configuration/configuration-types/#exporting-multiple-configurations)时，只考虑第一个配置的devServer选项，并将其用于数组中的所有配置。

> **\[info\]** 注：
>
> 如果您遇到麻烦，导航到`/webpack-dev-server`路由将显示被服务的文件的位置。例如,http://localhost:9000 / webpack-dev-server。



