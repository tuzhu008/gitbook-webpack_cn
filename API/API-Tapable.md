# Tapable

[Tapable](#) 是一个小型的库，允许您在javascript模块中添加和应用插件。它可以被继承或混合到其他模块中。它类似于NodeJS的`EventEmitter`类，主要关注自定义事件发射和操作。然而，除了这一点，`Tapable`允许你通过回调参数来访问事件的"emitter\(发射器）" 或 "producer（生产者）"。

`Tapable` 有四组成员函数:

* `plugin(name:string, handler:function)`：这允许一个自定义的插件注册到一个`Tapable实例`的事件中。这类似于`EventEmitter`的`on()`方法，它用于注册一个handler/listener，以便在信号/事件（signal/event ）发生时做一些事情。
* `apply(…pluginInstances: (AnyPlugin|function)[])` ：`AnyPlugin`应该是一个具有`apply`方法的类\(或者很少是一个对象\)，或者只是一个带有一些注册代码的函数。这个方法只是**应用**插件的定义，这样真正的事件监听器就可以注册到Tapable实例的注册表中。
* `applyPlugins*(name:string, …)`：Tapable实例可以使用这些函数将所有插件应用到特定的hash中。这组方法就像`EventEmitter`的`emit()`方法一样，通过各种策略精心地控制事件发射。
* `mixin(pt: Object)`：一种简单的方法来扩展（extend）`Tapable`的原型（prototype），它将作为一个混合而不是继承被扩展。

不同的`applyPlugins*`方法包括以下用例:

* 插件可以串联（serially）运行。
* 插件可以并行运行。
* 插件可以一个接一个地运行，但是从以前的插件\(瀑布waterfall\)中获取输入（将之前的插件结果作为参数）。
* 插件可以异步运行。
* 在bail上退出运行的插件：也就是说，一旦一个插件返回`非未定义`的状态，跳出运行流并返回那个插件的返回值。这听起来像是`EventEmitter`的`once()`，但却完全不同。

## 示例

webpack的可Tapable实例之一，[编译器](//API/API-Compiler.md)，负责编译webpack配置对象，并返回一个[编译](//API/API-Compilation.md)实例。当编译实例运行时，它会创建所需的bundle。

以下是如何使用`Tapable`的简化版本:

**node\_modules/webpack/lib/Compiler.js**

```js
var Tapable = require("tapable");

function Compiler() {
    Tapable.call(this);
}

Compiler.prototype = Object.create(Tapable.prototype);
```

现在在编译器上写一个插件

**my-custom-plugin.js**

```js
function CustomPlugin() {}
CustomPlugin.prototype.apply = function(compiler) {
  compiler.plugin('emit', pluginFunction);
}
```

编译器在其生命周期的适当点上执行该插件：

**node\_modules/webpack/lib/Compiler.js**

```js
this.apply*("emit",options) // 将会获取'emit'名字下的所有插件，并运行它们。
```



