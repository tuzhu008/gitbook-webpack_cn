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
> 在导出[多个配置](https://doc.webpack-china.org/configuration/configuration-types/#exporting-multiple-configurations)时，只考虑第一个配置的devServer选项，并将其用于数组中的所有配置。-

-

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

模仿django的`ALLOWED_HOSTS`，这是一个以`.`开始的值，可以用作子域名通配符。`.host.com`将匹配`host.com`、`www.host.com`和`host.com`的任何其他子域名。

```js
// 这与第一个例子的效果相同
// 无需更新配置就可以获得额外的好处
// 如果新子域需要访问开发服务器
allowedHosts: [
    '.host.com',
    'host2.com'
]
```

使用命令行接口CLI时，通过`--allowed-hosts`选择一个用逗号分隔的字符串。

```bash
webpack-dev-server --entry /entry/file --output-path /output/path --allowed-hosts .host.com,host2.com
```

### `devServer.before`

类型：function

提供了在服务器内部所有其他中间件之前执行定制中间件的能力。这可以用于定义自定义的处理程序（handler），例如:

```js
before(app){
  app.get('/some/path', function(req, res) {
    res.json({ custom: 'response' });
  });
}
```

### `devServer.bonjour`

这个选项配置在启动时通过ZeroConf网络广播服务器

```js
bonjour: true
```

通过CLI使用：

```js
webpack-dev-server --bonjour
```

### `devServer.clientLogLevel`

类型：string

当使用_内联模式\(inline mode\)_时，在开发工具\(DevTools\)的控制台\(console\)将显示消息，如：在重新加载之前，在一个错误之前，或者模块热替换\(Hot Module Replacement\)启用时。这可能显得很繁琐。

你可以阻止所有这些消息显示，使用这个选项：

```js
clientLogLevel: "none"
```

通过CLI使用：

```bash
webpack-dev-server --client-log-level none
```

可能的值有`none`,`error`,`warning`或者`info`（默认值）。

### `devServer.color`

**通过CLI设置。**

类型：boolean

在控制台显示/禁用颜色。

```bash
webpack-dev-server --color
```

### `devServer.compress`

类型：boolean

一切服务都启用[gzip 压缩](https://betterexplained.com/articles/how-to-optimize-your-site-with-gzip-compression/)：

```js
compress: true
```

通过CLI使用：

```js
webpack-dev-server --compress
```

### `devServer.contentBase`

类型：boolean \| string \| array

告诉服务器从哪里提供内容。只有在你想要提供静态文件时才需要。[`devServer.publicPath`](#devserverpublicpath-🔑)将用于确定应该从哪里提供 bundle，并且此选项优先。

默认情况下，将使用当前工作目录作为提供内容的目录，但是你可以修改为其他目录：

```js
contentBase: path.join(__dirname, "public")
```

> **\[info\]** 注：
>
> 推荐使用绝对路径。

但是也可以从多个目录提供内容：

```js
contentBase: [path.join(__dirname, "public"), path.join(__dirname, "assets")]
```

禁用`contentBase`：

```js
contentBase: false
```

通过CLI使用：

```js
webpack-dev-server --content-base /path/to/content/dir
```

### `devServer.disableHostCheck`

类型：boolean

当该选项设置为true时，将绕过主机检查。**这是不推荐的**，因为不检查主机的应用程序很容易受到DNS重新绑定攻击。

```js
disableHostCheck: true
```

通过CLI使用：

```js
webpack-dev-server --disable-host-check
```

### `devServer.filename 🔑`

类型：string

在**惰性模式**中，此选项可减少编译。 默认在**惰性模式**，每个请求结果都会产生全新的编译。使用`filename`，可以只在某个文件被请求时编译。

如果`output.filename`设置为`bundle.js`，`filename`使用如下：

```js
lazy: true,
filename: "bundle.js"
```

现在只有在请求`/bundle.js`时候，才会编译 bundle。

> **\[info\]** 注：
>
> `filename`在不使用**懒加载**时没有效果

### `devServer.headers 🔑`

类型：object

在所有响应中添加首部内容：

```js
headers: {
  "X-Custom-Foo": "bar"
}
```

### `devServer.historyApiFallback`

类型：boolean \| object

当使用[HTML5 History API](https://developer.mozilla.org/en-US/docs/Web/API/History)时，任意的`404`响应都可能需要被替代为`index.html`。通过传入以下启用：

```js
historyApiFallback: true
```

通过传入一个对象，比如使用`rewrites`这个选项，此行为可进一步地控制：

```js
historyApiFallback: {
  rewrites: [
    { from: /^\/$/, to: '/views/landing.html' },
    { from: /^\/subpage/, to: '/views/subpage.html' },
    { from: /./, to: '/views/404.html' }
  ]
}
```

当路径中使用点\(dot\)（常见于 Angular），你可能需要使用`disableDotRule`：

```js
historyApiFallback: {
  disableDotRule: true
}
```

通过CLI使用

```bash
webpack-dev-server --history-api-fallback
```

### `devServer.host`

类型：string

指定使用一个 host。默认是`localhost`。如果你希望服务器外部可访问，指定如下：

```js
host: "0.0.0.0"
```

通过CLI使用：

```js
webpack-dev-server --host 0.0.0.0
```

### `devServer.hot`

类型：boolean

启用 webpack 的模块热替换特性：

```js
hot: true
```

> **\[info\]** 注：
>
> `webpack.HotModuleReplacementPlugin`对于完全启用HMR是必须的。如果`webpack`或`webpack-devserver`是用`--hot`选项启动的，这个插件将会自动添加，因此您可能不需要将它添加到您的`webpack.config.js`中。请参阅[HMR概念页面](https://doc.webpack-china.org/concepts/hot-module-replacement)获得更多信息。

### `devServer.hotOnly`

类型：boolean

启用热模块替换\(见[`devServer.hot`](#devserverhot)\)，而不需要页面刷新，以防出现构建失败。

```js
hotOnly: true
```

通过CLI使用

```bash
webpack-dev-server --hot-only
```

### `devServer.https`

类型：boolean \| object

默认情况下，dev-server 通过 HTTP 提供服务。也可以选择带有 HTTPS 的 HTTP/2 提供服务：

```js
https: true
```

使用以下设置自签名证书，但是你可以提供自己的：

```js
https: {
  key: fs.readFileSync("/path/to/server.key"),
  cert: fs.readFileSync("/path/to/server.crt"),
  ca: fs.readFileSync("/path/to/ca.pem"),
}
```

此对象直接传递到 Node.js HTTPS 模块，所以更多信息请查看[HTTPS 文档](https://nodejs.org/api/https.html)。

通过CLI使用：

```bash
webpack-dev-server --https
```

要通过CLI使用您自己的证书，请使用以下选项：

```bash
webpack-dev-server --https --key /path/to/server.key --cert /path/to/server.crt --cacert /path/to/ca.pem
```

### `devServer.index`

类型：string

被认为是索引文件的文件名。

```js
index: 'index.htm'
```

### `devServer.info`

**只能通过CLI配置**

类型：boolean

输出CLI信息，默认是启用的。

```bash
webpack-dev-server --info=false
```

### `devServer.inline`

类型：boolean

在 dev-server 的两种不同模式之间切换。默认情况下，应用程序启用_内联模式\(inline mode\)_。这意味着一段处理实时重载的脚本被插入到你的包\(bundle\)中，并且构建消息将会出现在浏览器控制台。

也可以使用**iframe 模式**，它在通知栏下面使用`<iframe>`标签，包含了关于构建的消息。切换到**iframe 模式**：

```js
inline: false
```

通过命令行使用：

```bash
webpack-dev-server --inline=false
```

> **\[info\]** 注：
>
> 推荐使用模块热替换的内联模式，因为它包含来自 websocket 的 HMR 触发器。轮询模式可以作为替代方案，但需要一个额外的入口点：`'webpack/hot/poll?1000'`。

### `devServer.lazy 🔑`

类型：boolean

当启用`lazy`时，dev-server 只有在请求时才编译包\(bundle\)。这意味着 webpack 不会监视任何文件改动。我们称之为“**惰性模式**”。

```js
lazy: true
```

通过命令行使用：

```js
webpack-dev-server --lazy
```

> **\[info\]** 注：
>
> `watchOptions`在使用**惰性模式**时无效。

-

> **\[info\]** 注：
>
> 如果使用命令行工具\(CLI\)，请确保**内联模式\(inline mode\)**被禁用。

### `devServer.noInfo 🔑`

类型：boolean

启用`noInfo`后，诸如「启动时和每次保存之后，那些显示的 webpack 包\(bundle\)信息」的消息将被隐藏。错误和警告仍然会显示。

```js
noInfo: true
```

### `devServer.open`

类型：boolean

当`open`被启用，dev server将打开浏览器

```js
open: true
```

通过CLI使用:

```bash
webpack-dev-server --open
```

### `devServer.openPage`

类型：string

指定在打开浏览器时导航到的页面。

```js
openPage: '/different/page'
```

### `devServer.overlay`

类型：boolean \| object

当出现编译错误或警告时，在浏览器中显示一个全屏覆盖。默认情况下禁用。如果您只想显示编译器错误:

```js
overlay: true
```

如果您想要显示警告和错误:

```js
overlay: {
  warnings: true,
  errors: true
}
```

### `devServer.pfx`

类型：string

当通过CLI使用时，是一条到SSL`.pfx`文件的路径。如果在选项中使用，它应该是`.pfx`文件的字节流

```js
pfx: '/path/to/file.pfx'
```

通过CLI使用：

```bash
webpack-dev-server --pfx /path/to/file.pfx
```

### `devServer.pfxPassphrase`

类型：string

The passphrase to a SSL PFX file.

pfxPassphrase: 'passphrase'

通过CLI使用：

```bash
webpack-dev-server --pfx-passphrase passphrase
```

### `devServer.port`

类型：number

```js
port: 8080
```

通过CLI使用：

```bash
webpack-dev-server --port 8080
```

### `devServer.proxy`

类型：object

如果你有单独的后端开发服务器 API，并且希望在同域名下发送 API 请求 ，那么代理某些 URL 会很有用。

dev-server 使用了非常强大的[http-proxy-middleware](https://github.com/chimurai/http-proxy-middleware)包。更多高级用法，请查阅其[文档](https://github.com/chimurai/http-proxy-middleware#options)。

在`localhost:3000`上有后端服务的话，你可以这样启用代理：

```js
proxy: {
  "/api": "http://localhost:3000"
}
```

请求到`/api/users`现在会被代理到请求`http://localhost:3000/api/users`。

如果你不想始终传递`/api`，则需要重写路径：

```js
proxy: {
  "/api": {
    target: "http://localhost:3000",
    pathRewrite: {"^/api" : ""}
  }
}
```

默认情况下，不接受运行在 HTTPS 上，且使用了无效证书的后端服务器。如果你想要接受，修改配置如下：

```js
proxy: {
  "/api": {
    target: "https://other-server.example.com",
    secure: false
  }
}
```

有时你不想代理所有的请求。可以基于一个函数的返回值绕过代理。

在函数中你可以访问请求体、响应体和代理选项。必须返回`false`或路径，来跳过代理请求。

例如：对于浏览器请求，你想要提供一个 HTML 页面，但是对于 API 请求则保持代理。你可以这样做：

```js
proxy: {
  "/api": {
    target: "http://localhost:3000",
    bypass: function(req, res, proxyOptions) {
      if (req.headers.accept.indexOf("html") !== -1) {
        console.log("Skipping proxy for browser request.");
        return "/index.html";
      }
    }
  }
}
```

如果您想要代理多个特定路径到同一个目标，您可以使用一个或多个具有上下文属性的对象的数组:

```js
proxy: [{
  context: ["/auth", "/api"],
  target: "http://localhost:3000",
}]
```

### `devServer.progress`

**只能使用CLI配置**

类型：boolean

将运行进度输出到控制台。

```bash
webpack-dev-server --progress
```

### `devServer.public`

类型：string

当使用_内联模式\(inline mode\)_并代理 dev-server 时，内联的客户端脚本并不总是知道要连接到什么地方。它会尝试根据`window.location`来猜测服务器的 URL，但是如果失败，你需要这样。

例如，dev-server 被代理到 nginx，并且在`myapp.test`上可用：

```js
public: "myapp.test:80"
```

通过CLI使用：

```bash
webpack-dev-server --public myapp.test:80
```

### `devServer.publicPath 🔑`

类型：string

此路径下的打包文件可在浏览器中访问。

假设服务器运行在`http://localhost:8080`并且`output.filename`被设置为`bundle.js`。默认`publicPath`是`"/"`，所以你的包\(bundle\)可以通过`http://localhost:8080/bundle.js`访问。

可以修改`publicPath`，将 bundle 放在一个目录：

```js
publicPath: "/assets/"
```

你的包现在可以通过`http://localhost:8080/assets/bundle.js`访问。

> **\[info\]** 注：
>
> 确保`publicPath`总是以斜杠\(/\)开头和结尾。

也可以使用一个完整的 URL。这是模块热替换所必需的。

```js
publicPath: "http://localhost:8080/assets/"
```

bundle 可以通过`http://localhost:8080/assets/bundle.js`访问。

> **\[info\]** 注：
>
> 推荐使用`devServer.publicPath`和`output.publicPath`

### `devServer.quiet 🔑`

类型：boolean

启用`quiet`后，除了初始启动信息之外的任何内容都不会被打印到控制台。这也意味着来自 webpack 的错误或警告在控制台不可见。

```js
quiet: true
```

通过CLI使用

```bash
webpack-dev-server --quiet
```

### `devServer.socket`

类型：string

The Unix socket to listen to \(instead of a host\).

```
socket:'socket'
```

通过 CLI使用：

```
webpack-dev-server --socket socket
```

### `devServer.staticOptions`

可以配置来自`contentBase`的提供静态文件的高级选项。关于可能的选项，请参阅[Express 文档](http://expressjs.com/en/4x/api.html#express.static)。一个例子:

```
staticOptions: {
  redirect: false
}
```

> **\[info\]** 注：
>
> 仅当使用`contentBase`作为一个`string`有效。

### `devServer.stats 🔑`

类型：string \| object

这个选项允许您精确控制哪些bundle信息被显示。如果你想要一些bundle信息，但不是全部，这可能是一个很好的中间地带。

这样只会显示包中的错误:

```js
stats: "errors-only"
```

For more information, see the**stats documentation**.

### `devServer.stdin - CLI only`

### `devServer.useLocalIp`

### `devServer.watchContentBase`

### `devServer.watchOptions 🔑`



