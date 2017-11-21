## Plugins API （插件API）

> **\[info\]** 注：
>
> 对于编写插件的高级介绍，从[编写插件](https://doc.webpack-china.org/contribute/writing-a-plugin/)开始。

webpack中的许多对象扩展了`Tapable类`，它暴露了一个`plugin`方法。使用`plugin`方法，插件可以注入自定义的构建步骤。您将看到`compiler.plugin`和`compilation.plugin`被使用很多。实际上，每一个插件都调用一个回调来在整个构建过程中对特定的步骤进行烧制（fire）。

有两种类型的插件接口:

**基于时序（Timing Based）**

* sync \(默认\): 该插件同步运行并返回其输出。
* async: 这个插件是异步运行的，并使用`callback`来返回它的输出。
* parallel: 处理程序是并行调用的。

**返回值（Return Value）**

* not bailing \(默认\)：没有返回值
* bailing：处理程序将被调用，直到一个处理程序返回某个东西。
* parallel bailing：处理程序是并行调用的\(async\)。第一个返回值\(按顺序\)是非常重要的。
* waterfall：每个处理程序获取上一个处理程序的结果值作为参数。

A plugin is installed once as webpack starts up. webpack installs a plugin by calling its `apply` method, and passes a reference to the webpack `compiler` object. You may then call `compiler.plugin` to access asset compilations and their individual build steps. An example would look like this:

一旦webpack启动，就会安装一个插件。webpack通过调用它的`apply`方法来安装插件，并将一个引用传递给webpack`compiler（编译器）`对象。然后可以调用`compiler.plugin`来访问资产编译及其单独构建步骤。一个例子是这样的:

**my-plugin.js**

```js
function MyPlugin(options) {
  // 使用一些选项来配置你的插件...
}

MyPlugin.prototype.apply = function(compiler) {
  compiler.plugin("compile", function(params) {
    console.log("The compiler is starting to compile...");
  });

  compiler.plugin("compilation", function(compilation) {
    console.log("The compiler is starting a new compilation...");

    compilation.plugin("optimize", function() {
      console.log("The compilation is starting to optimize files...");
    });
  });

  compiler.plugin("emit", function(compilation, callback) {
    console.log("The compilation is going to emit files...");
    callback();
  });
};

module.exports = MyPlugin;
```

**webpack.config.js**

```js
plugins: [
  new MyPlugin({
    options: 'nada'
  })
]
```

## Tapable & Tapable 实例

webpack的插件架构大体上是合理的，这要归功于一个名为`Tapable`的内部库，  
**Tapable 实例** 是webpack源代码中的类，它们已经被扩展或混合在类`Tapable`中。

对于插件作者来说，重要的是要知道在webpack源代码中哪些是`Tapable`的实例。这些实例提供了各种各样的事件钩子到可以被附加自定义的插件中。因此，贯穿本部分的是所有webpack `Tapable`实例\(以及它们的事件钩子\)的列表，这些插件的作者可以利用它们。

有关可访问的更多信息，请访问[完整的概述](//API/API-Tapable.md)或可访问[tapable 库](https://github.com/webpack/tapable)。

