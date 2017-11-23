<div align="center">
  <a href="https://offline-plugin.now.sh/"><img src="https://rawgit.com/NekR/offline-plugin/master/logo/logo.svg" width="120" alt="offline-plugin logo"></a>
</div>

<h1><code>offline-plugin</code> for webpack</h1>

[![](https://img.shields.io/badge/Github-%E6%9F%A5%E7%9C%8B%E6%9B%B4%E5%A4%9A-brightgreen.svg)
](https://github.com/NekR/offline-plugin)


这个插件旨在为**webpack**项目提供离线体验。它使用**ServiceWorker**，而**AppCache**作为底层的备用。简单地把这个插件包含在你的``webpack.config``，以及在客户端脚本中附带运行时（runtime），您的项目将通过缓存所有(或部分)webpack输出资源来离线。

<div>
  <strong>示例:<br><a href="https://offline-plugin.now.sh/"> 用<code>offline-plugin</code>构建的渐进式Web应用</a></strong><br> 
  <div>(<a href="https://github.com/NekR/offline-plugin-pwa"><i>源码</i></a>)</div>
</div>

## 安装

`npm install offline-plugin [--save-dev]`

## 设置

首先，在`webpack.config`中使用[options](docs/options.md)实例化插件：

```js
// webpack.config.js 

var OfflinePlugin = require('offline-plugin');

module.exports = {
  // ...

  plugins: [
    // ... 其他插件
    // 如果OfflinePlugin是最后一个被添加的插件，那就更好了
    new OfflinePlugin()
  ]
  // ...
}

```

然后添加 [运行时](docs/runtime.md)到你的入口文件(主入口):

```js
require('offline-plugin/runtime').install();
```

ES6/Babel/TypeScript
```js
import * as OfflinePluginRuntime from 'offline-plugin/runtime';
OfflinePluginRuntime.install();
```

> **[info]** 注：
> 
> 关于 `TypeScript`的更多使用细节，[请参见](docs/typescript.md)

## Docs

* [Caches](docs/caches.md)
* [Update process](docs/updates.md)
* [Cache Maps](docs/cache-maps.md)
* [Runtime API](docs/runtime.md)
* [Configuration options](docs/options.md)
* [FAQ](docs/FAQ.md)
* [Troubleshooting](docs/troubleshooting.md)

## 示例

* [单页应用](docs/examples/SPA.md)

## 文章

* [使用Webpack的Offline Plugin轻松离线的第一款应用](https://dev.to/kayis/easy-offline-first-apps-with-webpacks-offline-plugin)
* [处理客户端应用程序更新(使用Service Workers)](https://zach.codes/handling-client-side-app-updates-with-service-workers/)

## 选项

所有选项都是可选的，而`offline-plugin`可以在不指定任何选项的情况下使用。

[查看所有可用选项。](docs/options.md)

## 谁在使用 `offline-plugin`

### 项目

* [React Boilerplate](https://github.com/mxstbr/react-boilerplate)
* [Phenomic](https://phenomic.io)
* [React, Universally](https://github.com/ctrlplusb/react-universally)

### 渐进式应用（PWA）

* [`offline-plugin` PWA](https://offline-plugin.now.sh/)
* [Offline Kanban](https://offline-kanban.herokuapp.com) ([source](https://github.com/sarmadsangi/offline-kanban))
* [Preact](https://preactjs.com/) ([source](https://github.com/developit/preact-www))
* [Omroep West (_Proof of Concept_)](https://omroep-west.now.sh/)
* [Online Board](https://onlineboard.sonnywebdesign.com/) ([source](https://github.com/andreasonny83/online-board))
* [CodePan](https://codepan.net) ([source](https://github.com/egoist/codepan))


_如果你正在使用 `offline-plugin`, 请随意提交一份PR，将你的项目添加到这个列表中。._
