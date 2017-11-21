# Resolver（解析器）

有三种类型的解析器，它们分别用于不同类型的模块:

* `compiler.resolvers.normal`：解析普通模块。
* `compiler.resolvers.context`：解析上下文模块。
* `compiler.resolvers.loader`：解析loader

任何插件都应该使用`this.fileSystem`作为文件系统，因为它会被缓存。它只有异步的命名函数，但是如果用户使用同步文件系统实现\(在enhanced-require中\)，它们可能表现同步。

To join paths any plugin should use `this.join`. It normalizes the paths. There is a `this.normalize` too.要连接路径，任何插件都应该使用`this.join`。它可序列化路径。也有一个`this.normalize`。

在`this.forEachBail(array, iterator, callback)`中可以使用一个bailing的异步`forEach`实现

要将请求传递给其他解析插件，请使用`this.doResolve(types: String|String[], request: Request, message: String, callback)`方法。`types`是多种可能的请求类型，它们是按照优先级进行测试的。

```js
interface Request {
  path: String // 请求的当前目录
  request: String // 当前请求字符串
  query: String // 请求的查询字符串，如果有的话
  module: boolean // 请求从一个模块开始
  directory: boolean // 请求指向一个目录
  file: boolean // 请求指向一个文件
  resolved: boolean //  请求被解决或已完成
  // undefine表示布尔字段为false
}

// 示例
// from /home/user/project/file.js: require("../test?charset=ascii")
{
  path: "/home/user/project",
  request: "../test",
  query: "?charset=ascii"
}
// from /home/user/project/file.js: require("test/test/")
{
  path: "/home/user/project",
  request: "test/test/",
  module: true,
  directory: true
}
```

## `resolve(context: String, request: String)`

解决过程开始之前。

## `resolve-step(types: String[], request: Request)`

在解析过程中单步执行之前。

## `module(request: Request)` async waterfall

找到一个模块请求，并且应该得到解决。

## `directory(request: Request)` async waterfall

找到一个目录请求，并且应该得到解决。

## `file(request: Request)` async waterfall

找到一个文件请求，并且应该得到解决。

## The plugins may offer more extensions points

下面列出了webpack提供的默认插件。They are all `(request: Request)` async waterfall.它们都是`(request: Request)`异步瀑布。

The process for normal modules and contexts is `module -> module-module -> directory -> file`.

The process for loaders is `module -> module-loader-module -> module-module -> directory -> file`.

## `module-module`

A module should be looked up in a specified directory. `path` contains the directory.

## `module-loader-module` \(only for loaders\)

Used before module templates are applied to the module name. The process continues with `module-module`.

---

> 原文：[https://webpack.js.org/api/plugins/resolver/](https://webpack.js.org/api/plugins/resolver/)



