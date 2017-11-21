# Compilation（编译）

Compilation 实例继承于 compiler。例如，`compiler.compilation` 是对所有 require 图\(graph\)中对象的字面上的编译。这个对象可以访问所有的模块和它们的依赖（大部分是间接依赖）。在编译阶段，模块被加载，密封，优化，分块（chunked），哈希化（hashed）和重建等等。这将是编译中任何操作主要的生命周期。

```js
compiler.plugin("compilation", function(compilation) {
    // 主编译实例
    // 后续的方法都派生自 compilation.plugin 
});
```

## `normal-module-loader`

`normal-module-loader`是一个函数，它是实际上加载模块图\(graph\)中所有的模块的函数（一个接一个）。

```js
compilation.plugin('normal-module-loader', function(loaderContext, module) {
  // 这里是所有模块被加载的地方
  // 一个接一个，此时还没有依赖被创建
});
```

## `seal`

编译的密封已经开始。

```js
compilation.plugin('seal', function() {
  // 你已经不能再接收到任何模块
  // 没有参数
});
```

## `optimize`

优化编译。

```js
compilation.plugin('optimize', function() {
  // webpack 已经进入优化阶段
  // 没有参数
});
```

## `optimize-tree(chunks, modules)` 异步

树的异步优化。

```js
compilation.plugin('optimize-tree', function(chunks, modules) {

});
```

### `optimize-modules(modules: Module[])`

模块的优化。

```js
compilation.plugin('optimize-modules', function(modules) {
  // 树优化期间处理模块数组
});
```

## `after-optimize-modules(modules: Module[])`

模块优化已经结束。

## `optimize-chunks(chunks: Chunk[])`

块的优化。

```javascript
// 块优化可以在编译过程中运行多次

compilation.plugin('optimize-chunks', function(chunks) {
    // 这里一般只有一个块，
    // 除非你在配置中指定了多个入口
    chunks.forEach(function (chunk) {
        // 块有对其模块的间接引用
        chunk.modules.forEach(function (module){
            // module.loaders, module.rawRequest, module.dependencies, 等等.
        });
    });
});
```

## `after-optimize-chunks(chunks: Chunk[])`

块优化完成。

## `revive-modules(modules: Module[], records)`

从记录中重建模块信息。

## `optimize-module-order(modules: Module[])`

按重要性对模块进行排序。第一个是最重要的模块。它会得到最小的id值。

## `optimize-module-ids(modules: Module[])`

优化模块id。

## `after-optimize-module-ids(modules: Module[])`

模块id优化完成。

## `record-modules(modules: Module[], records)`

将模块信息存储到记录中。

## `revive-chunks(chunks: Chunk[], records)`

从记录中重建块信息。

## `optimize-chunk-order(chunks: Chunk[])`

把这些块按重要性排序。第一个是最重要的块。它会得到最小的id。

## `optimize-chunk-ids(chunks: Chunk[])`

优化块的id。

## `after-optimize-chunk-ids(chunks: Chunk[])`

块的id优化完成。

## `record-chunks(chunks: Chunk[], records)`

将块信息存储到记录中。

## `before-hash`

在编译被哈希化之前。

## `after-hash`

在编译被哈希化之后。

## `before-chunk-assets`

在创建块资源之前。

## `additional-chunk-assets(chunks: Chunk[])`

为块创建额外的资源。

## `record(compilation, records)`

将编译的信息存储到记录中

## `additional-assets` async

为编译创建额外的资源。

下面是一个下载图像的示例。

```js
compiler.plugin('compilation', function(compilation) {
  compilation.plugin('additional-assets', function(callback) {
    download('https://img.shields.io/npm/v/webpack.svg', function(resp) {
      if(resp.status === 200) {
        compilation.assets['webpack-version.svg'] = toAsset(resp);
        callback();
      } else {
        callback(new Error('[webpack-example-plugin] Unable to download the image'));
      }
    })
  });
});
```

## `optimize-chunk-assets(chunks: Chunk[])` async

优化 chunk 的资源。

资源被存储在 `this.assets`，但是它们并不都是块的资源。一个 `Chunk` 有一个 `files` 属性指向由这个块创建的所有文件。额外的资源被存储在 `this.additionalChunkAssets` 中。

这是一个为每个 chunk 添加 banner 的例子。

```js
compilation.plugin("optimize-chunk-assets", function(chunks, callback) {
  chunks.forEach(function(chunk) {
    chunk.files.forEach(function(file) {
      compilation.assets[file] = new ConcatSource("\/**Sweet Banner**\/", "\n", compilation.assets[file]);
    });
  });
  callback();
});
```

## `after-optimize-chunk-assets(chunks: Chunk[])`

块资源已经被优化。这里是一个来自 [@boopathi](https://github.com/boopathi) 的示例插件，详细的输出每个块里有什么。

```js
var PrintChunksPlugin = function() {};

PrintChunksPlugin.prototype.apply = function(compiler) {
  compiler.plugin('compilation', function(compilation, params) {
    compilation.plugin('after-optimize-chunk-assets', function(chunks) {
      console.log(chunks.map(function(c) {
        return {
          id: c.id,
          name: c.name,
          includes: c.modules.map(function(m) {
            return m.request;
          })
        };
      }));
    });
  });
};
```

## `optimize-assets(assets: Object{name: Source})` async

优化所有资源。

资源被存放在 `this.assets`。

## `after-optimize-assets(assets: Object{name: Source})`

资源优化已经结束。

## `build-module(module)`

一个模块构建开始前。

```js
compilation.plugin('build-module', function(module){
  console.log('About to build: ', module);
});
```

## `succeed-module(module)`

一个模块已经被成功构建。

```js
compilation.plugin('succeed-module', function(module){
  console.log('Successfully built: ', module);
});
```

## `failed-module(module)`

一个模块构建失败。

```js
compilation.plugin('failed-module', function(module){
  console.log('Failed to build: ', module);
});
```

## `module-asset(module, filename)`

一个模块中的一个资源被加到编译中。

## `chunk-asset(chunk, filename)`

一个 chunk 中的一个资源被加到编译中。



