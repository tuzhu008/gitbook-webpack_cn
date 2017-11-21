# Compiler（编译器）

The `Compiler` module of webpack is the main engine that creates a compilation instance with all the options passed through webpack CLI or `webpack` api or webpack configuration file.

It is exported by `webpack` api under `webpack.Compiler`.

The compiler is used by webpack by instantiating it and then calling the `run` method. Below is a trivial example of how one might use the `Compiler`. In fact, this is really close to how webpack itself uses it.

webpack的`Compiler`模块**是**通过webpack CLI或`webpack` api或webpack配置文件传递的所有选项创建一个**编译实例**的**主引擎**（engine）。

它是由`webpack.Compiler`下的**webpack** api导出的。

webpack通过实例化它，然后调用`run`方法来使用编译器。下面是一个关于如何使用`Compiler`的简单示例。事实上，这与webpack本身的使用方式非常接近。

[**compiler-example**](https://github.com/pksjce/webpack-internal-examples/tree/master/compiler-example)

```javascript
// Can be imported from webpack package
import {Compiler} from 'webpack';

// Create a new compiler instance
const compiler = new Compiler();

// Populate all required options
compiler.options = {...};

// Creating a plugin.
class LogPlugin {
  apply (compiler) {
    compiler.plugin('should-emit', compilation => {
      console.log('should I emit?');
      return true;
    })
  }
}

// Apply the compiler to the plugin
new LogPlugin().apply(compiler);

/* Add other supporting plugins */

// Callback to be executed after run is complete
const callback = (err, stats) => {
  console.log('Compiler has finished execution.');
  // Display stats...
};

// call run on the compiler along with the callback
compiler.run(callback);
```

The `Compiler` is what we call a `Tapable` instance. By this, we mean that it mixes in `Tapable` class to imbibe functionality to register and call plugins on itself. Most user facing plugins are first registered on the `Compiler`. The working of a Compiler can be condensed into the following highlights

* Usually there is one master instance of Compiler. Child compilers can be created for delegating specific tasks.
* A lot of the complexity in creating a compiler goes into populating all the relevant options for it.
* `webpack` has [`WebpackOptionsDefaulter`](https://github.com/webpack/webpack/blob/master/lib/WebpackOptionsDefaulter.js) and [`WebpackOptionsApply`](https://github.com/webpack/webpack/blob/master/lib/WebpackOptionsApply.js) specifically designed to provide the `Compiler` with all the initial data it requires.
* The `Compiler` is ultimately just a function which performs bare minimum functionality to keep a lifecycle running. It delegates all the loading/bundling/writing work to various plugins.
* `new LogPlugin(args).apply(compiler)` registers the plugin to any particular hook event in the `Compiler`'s lifecycle.
* The `Compiler` exposes a `run` method which kickstarts all compilation work for `webpack`. When that is done, it should call the passed in `callback` function. All the tail end work of logging stats and errors are done in this callback function.

## Watching

The `Compiler` supports "watch mode" which monitors the file system and recompiles as files change. When in watch mode, the compiler will emit the additional events ["watch-run", "watch-close", and "invalid"](#event-hooks). This is typically used in [development](/guides/development), usually under the hood of tools like `webpack-dev-server`, so that the developer doesn't need to re-compile manually every time.

For more details about watch mode, see the [Node.js API documentation](/api/node/#watching) or the [CLI watch options](/api/cli/#watch-options).

## MultiCompiler

This module, MultiCompiler, allows webpack to run multiple configurations in separate compiler.  
If the `options` parameter in the webpack's NodeJS api is an array of options, webpack applies separate compilers and calls the `callback` method at the end of each compiler execution.

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

## Event Hooks

This a reference guide to all the event hooks exposed by the `Compiler`.

| Event name | Reason | Params | Type |
| --- | --- | --- | --- |
| `entry-option` | - | - | bailResult |
| `after-plugins` | After setting up initial set of plugins | `compiler` | sync |
| `after-resolvers` | After setting up the resolvers | `compiler` | sync |
| `environment` | - | - | sync |
| `after-environment` | Environment setup complete | - | sync |
| `before-run` | `compiler.run()` starts | `compiler` | async |
| `run` | Before reading records | `compiler` | async |
| `watch-run` | Before starting compilation after watch | `compiler` | async |
| `normal-module-factory` | After creating a `NormalModuleFactory` | `normalModuleFactory` | sync |
| `context-module-factory` | After creating a `ContextModuleFactory` | `contextModuleFactory` | sync |
| `before-compile` | Compilation parameters created | `compilationParams` | async |
| `compile` | Before creating new compilation | `compilationParams` | sync |
| `this-compilation` | Before emitting `compilation` event | `compilation` | sync |
| `compilation` | Compilation creation completed | `compilation` | sync |
| `make` | - | `compilation` | parallel |
| `after-compile` | - | `compilation` | async |
| `should-emit` | Can return true/false at this point | `compilation` | bailResult |
| `need-additional-pass` | - | - | bailResult |
| `emit` | Before emitting assets to output dir | `compilation` | async |
| `after-emit` | After emitting assets to output dir | `compilation` | async |
| `done` | Completion of compile | `stats` | sync |
| `failed` | Failure of compile | `error` | sync |
| `invalid` | After invalidating a watch compile | `fileName`, `changeTime` | sync |
| `watch-close` | After stopping a watch compile | - | sync |

## Usage

Here's an example of an asynchronous `emit` event handler:

```javascript
compiler.plugin("emit", function(compilation, callback) {
  // Do something async...
  setTimeout(function() {
    console.log("Done with async work...");
    callback();
  }, 1000);
});
```



