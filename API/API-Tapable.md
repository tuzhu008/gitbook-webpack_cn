# Tapable

[Tapable](#) 是一个小型的库，允许您在javascript模块中添加和应用插件。它可以被继承或混合到其他模块中。它类似于NodeJS的`EventEmitter`类，主要关注自定义事件发射和操作。然而，除了这一点，`Tapable`允许你通过回调参数来访问事件的"emitter\(发射器）" 或 "producer（生产者）"。

`Tapable` 有四组成员函数:

* `plugin(name:string, handler:function)`：这允许一个自定义的插件注册到一个`Tapable实例`的事件中。这类似于`EventEmitter`的`on()`方法，它用于注册一个handler/listener，以便在信号/事件（signal/event ）发生时做一些事情。
* `apply(…pluginInstances: (AnyPlugin|function)[])` ：`AnyPlugin`应该是一个具有`apply`方法的类\(或者很少是一个对象\)，或者只是一个带有一些注册代码的函数。这个方法只是**应用**插件的定义，这样真正的事件监听器就可以注册到Tapable实例的注册表中。
* `applyPlugins*(name:string, …)`：Tapable实例可以使用这些函数将所有插件应用到特定的hash中。这组方法就像`EventEmitter`的`emit()`方法一样，通过各种策略精心地控制事件发射。
* `mixin(pt: Object)`：一种简单的方法来扩展（extend）`Tapable`的原型（prototype），它将作为一个混合而不是继承被扩展。

The different `applyPlugins*` methods cover the following use cases:

* Plugins can run serially.
* Plugins can run in parallel.
* Plugins can run one after the other but taking input from the previous plugin \(waterfall\).
* Plugins can run asynchronously.
* Quit running plugins on bail: that is, once one plugin returns non-`undefined`, jump out of the run flow and return _the return of that plugin_. This sounds like `once()` of `EventEmitter` but is totally different.

## Example

One of webpack's _Tapable_ instances, [Compiler](/api/compiler), is responsible for compiling the webpack configuration object and returning a [Compilation](/api/compilation) instance. When the Compilation instance runs, it creates the required bundles.

See below for a simplified version of how this looks using `Tapable`:

**node\_modules/webpack/lib/Compiler.js**

```js
var Tapable = require("tapable");

function Compiler() {
    Tapable.call(this);
}

Compiler.prototype = Object.create(Tapable.prototype);
```

Now to write a plugin on the compiler,

**my-custom-plugin.js**

```js
function CustomPlugin() {}
CustomPlugin.prototype.apply = function(compiler) {
  compiler.plugin('emit', pluginFunction);
}
```

The compiler executes the plugin at the appropriate point in its lifecycle by

**node\_modules/webpack/lib/Compiler.js**

```js
this.apply*("emit",options) // will fetch all plugins under 'emit' name and run them.
```



