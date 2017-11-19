# DevServer（开发服务）

webpack-dev-server 能够用于快速开发应用程序。

此页面描述影响 webpack-dev-server\(简写为：dev-server\) 行为的选项。

> **\[info\] 注：**
>
> 与[webpack-dev-middleware](https://github.com/webpack/webpack-dev-middleware)兼容的选项旁边有 🔑

```js
module.exports = {
    devServer: {
        after: '',
        
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
> 如果您遇到麻烦，导航到`/webpack-dev-server`路由将显示被服务的文件的位置。例如,`http://localhost:9000/webpack-dev-server`

### `devServer.after`

类型：function

提供了在服务器内部执行所有其他中间件之后执行定制中间件的能力。

```js
after(app){
  // 执行一些操作
}
```

### `devServer.allowedHosts`

类型：数组

这个选项允许你指定一个白名单，这个白名单记录了被允许访问的开发服务器。

```js
allowedHosts: [
  'host.com',
  'subdomain.host.com',
  'subdomain2.host.com',
  'host2.com'
]
```

模仿django的ALLOWED\_HOSTS，这是一个以`.`开始的值，可以用作子域名通配符。`.host.com`将匹配`host.com`、`www.host.com`和`host.com`的任何其他子域名。

```js
// 这与第一个例子的效果相同
// 无需更新配置就可以获得额外的好处
// 如果新子域需要访问开发服务器
allowedHosts: [
    '.host.com',
    'host2.com'
]
```

### `devServer.before`

### `devServer.bonjour`

### `devServer.clientLogLevel`

### `devServer.color - CLI only`

### `devServer.compress`

### `devServer.contentBase`

### `devServer.disableHostCheck`

### `devServer.filename 🔑`

### `devServer.headers 🔑`

### `devServer.historyApiFallback`

### `devServer.host`

### `devServer.hot`

### `devServer.hotOnly`

### `devServer.https`

### `devServer.index`

### `devServer.info - CLI only`

### `devServer.inline`

### `devServer.lazy 🔑`

### `devServer.noInfo 🔑`

### `devServer.open`

### `devServer.openPage`

### `devServer.overlay`

### `devServer.pfx`

### `devServer.pfxPassphrase`

### `devServer.port`

### `devServer.proxy`

### `devServer.progress - 只用于命令行工具(CLI)`

### `devServer.public`

### `devServer.publicPath 🔑`

### `devServer.quiet 🔑`

### `devServer.setup`

### `devServer.socket`

### `devServer.staticOptions`

### `devServer.stats 🔑`

### `devServer.stdin - CLI only`

### `devServer.useLocalIp`

### `devServer.watchContentBase`

### `devServer.watchOptions 🔑`



