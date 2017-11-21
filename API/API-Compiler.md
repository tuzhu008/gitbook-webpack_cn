# Compiler（编译器）

webpack的`Compiler`模块**是**通过webpack CLI或`webpack` api或webpack配置文件传递的所有选项创建一个**编译实例**的**主引擎**（engine）。

它是由`webpack.Compiler`下的**webpack** api导出的。

webpack通过实例化它，然后调用`run`方法来使用编译器。下面是一个关于如何使用`Compiler`的简单示例。事实上，这与webpack本身的使用方式非常接近。

[**compiler-example**](https://github.com/pksjce/webpack-internal-examples/tree/master/compiler-example)

```javascript
// 可以从webpack包中导入
import {Compiler} from 'webpack';

// 创建一个新的编译器实例
const compiler = new Compiler();

// 填充所有必需的选项
compiler.options = {...};

// 创建一个插件
class LogPlugin {
  apply (compiler) {
    compiler.plugin('should-emit', compilation => {
      console.log('should I emit?');
      return true;
    })
  }
}

// 将编译器应用到插件中
new LogPlugin().apply(compiler);

/* 添加其他支持插件 */

// run 后执行的完整回调
const callback = (err, stats) => {
  console.log('Compiler has finished execution.');
  // 显示统计数据（stats）……
};

// 在编译器和回调中调用 run
compiler.run(callback);
```

`Compiler`就是我们所称的`Tapable`实例。我们的意思是，它混合了`Tapable`的类来吸收功能来在它自己上注册和调用插件。大多数用户面对第一次在`Compiler`上注册的插件。编译器的工作可以被浓缩成以下要点：

* 通常有一个编译器的主实例。可以创建子编译器来委派特定的任务。

* 创建编译器的许多复杂性都是为了填充它的所有相关选项。

* `webpack`的[`WebpackOptionsDefaulter`](https://github.com/webpack/webpack/blob/master/lib/WebpackOptionsDefaulter.js) 和[`WebpackOptionsApply`](https://github.com/webpack/webpack/blob/master/lib/WebpackOptionsApply.js)专门用于提供带有所需的所有初始数据的`Compiler`。

* `Compiler`根本上只是一个函数，它执行最小的功能来维持生命周期的运行。它将所有的loading/bundling/writing工作委派给各种插件。

* `new LogPlugin(args).apply(compiler)`将插件注册到`Compiler`的生命周期中任何一个特定的钩子事件。

* `Compiler`暴露一个`run`方法，该方法启动了`webpack`的所有编译工作。完成后，它应该调用被传递的`callback`。日志统计和错误的所有收尾工作都在这个回调函数中完成。

## 观察（watching）

`Compiler`支持监视文件系统的“观察模式（watch mode）”，并在文件系统更改时重新编译。在观察模式下时，编译器将会发出额外的事件["watch-run", "watch-close", 和 "invalid"](#event-hooks)。这通常在开发中使用，通常是在`webpack-dev-server`等工具的底层，因此开发人员不需要每次都手动重新编译。

有关观察模式的更多细节，请参见[Node.js API 文档](#观察（watching）)或者[CLI watch options。](/configuration/watchOptions.md)

## 多变异器（MultiCompiler）

这个模块，多编译器，允许webpack在单独的编译器中运行多个配置。

如果webpack的NodeJS api中的`options`参数是options数组，webpack将为数组的每一项应用独立的编译器，并在每次编译器执行结束时调用`callback`。

```javascript
var webpack = require('webpack');

var config1 = {
  entry: './index1.js',
  output: {filename: 'bundle1.js'}
}
var config2 = {
  entry: './index2.js',
  output: {filename:'bundle2.js'}
}

webpack([config1, config2], (err, stats) => {
  process.stdout.write(stats.toString() + "\n");
})
```

## 事件钩子（Event Hooks）

这是编译`Compiler`暴露的的所有事件钩子的参考指南。

| Event name | Reason | Params | Type |
| --- | --- | --- | --- |
| `entry-option` | - | - | bailResult |
| `after-plugins` | 在设置了初始的插件集之后 | `compiler` | sync |
| `after-resolvers` | 在设置了解析器（resolver）之后 | `compiler` | sync |
| `environment` | - | - | sync |
| `after-environment` | 环境设置完成 | - | sync |
| `before-run` | `compiler.run() 开始` | `compiler` | async |
| `run` | 在阅读记录之前 | `compiler` | async |
| `watch-run` | 观察之后，编译之前 | `compiler` | async |
| `normal-module-factory` | 创建`NormalModuleFactory之后` | `normalModuleFactory` | sync |
| `context-module-factory` | 创建`ContextModuleFactory 之后` | `contextModuleFactory` | sync |
| `before-compile` | 编译参数创建完成 | `compilationParams` | async |
| `compile` | 创建一个新的编译之前 | `compilationParams` | sync |
| `this-compilation` | 发送`compilation`事件之前 | `compilation` | sync |
| `compilation` | 编译（Compilation）创建完成 | `compilation` | sync |
| `make` | - | `compilation` | parallel |
| `after-compile` | - | `compilation` | async |
| `should-emit` | 此时可以返回true/false | `compilation` | bailResult |
| `need-additional-pass` | - | - | bailResult |
| `emit` | 在将资产发送到输出目录之前 | `compilation` | async |
| `after-emit` | 将资产发送到输出目录之后 | `compilation` | async |
| `done` | 完成编译 | `stats` | sync |
| `failed` | 编译失败 | `error` | sync |
| `invalid` | 在观察编译失效后 | `fileName`, `changeTime` | sync |
| `watch-close` | 在停止观察编译之后 | - | sync |

## 用法

下面是一个异步`emit`事件处理程序的例子:

```javascript
compiler.plugin("emit", function(compilation, callback) {
  // Do something async...
  setTimeout(function() {
    console.log("Done with async work...");
    callback();
  }, 1000);
});
```



