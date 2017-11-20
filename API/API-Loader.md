# Loader API

所谓 loader 只是导出一个函数的 JavaScript 模块。[loader runner](https://github.com/webpack/loader-runner)会调用这个函数，然后把上一个 loader 产生的结果或者资源文件\(resource file\)传入进去。函数的`this`上下文将由 webpack 填充，并且[loader runner](https://github.com/webpack/loader-runner)带有一些有用方法，可以使 loader 改变为异步调用方式，或者获取 query 参数。

第一个 loader 的传入参数只有一个：资源文件\(resource file\)的内容。编译器需要得到最后一个 loader 产生的处理结果。这个处理结果应该是`String`或者`Buffer`（被转换为一个 string），代表了模块的 JavaScript 源码。另外还可以传递一个可选的 SourceMap 结果（格式为 JSON 对象）。

如果是单个处理结果，可以在**同步模式**中直接返回。如果有多个处理结果，则需要调用`this.callback()`。在**异步模式**中，必须调用`this.async()`，来指示[loader runner](https://github.com/webpack/loader-runner)等待异步结果，它会返回`this.callback()`回调函数，随后 loader 必须返回`undefined`并且调用该回调函数。

## 示例

### 同步 loader

**sync-loader.js**

```js
module.exports = function(content) {
  return someSyncOperation(content);
};
```

**sync-loader-with-multiple-results.js**

```js
module.exports = function(content) {
  this.callback(null, someSyncOperation(content), sourceMaps, ast);
  return; // 当调用callback()时总是返回undefined
};
```

### 异步 loader

**async-loader.js**

```js
module.exports = function(content) {
    var callback = this.async();
    someAsyncOperation(content, function(err, result) {
        if(err) return callback(err);
        callback(null, result);
    });
};
```

**async-loader-with-multiple-results.js**

```js
module.exports = function(content) {
    var callback = this.async();
    someAsyncOperation(content, function(err, result, sourceMaps, ast) {
        if(err) return callback(err);
        callback(null, result, sourceMaps, ast);
    });
};
```

> **\[info\]** 注：
>
> loader 最初被设计为可以在同步 loader pipeline（如 Node.js ，使用[enhanced-require](https://github.com/webpack/enhanced-require)），与异步 pipeline（如 webpack ）中运行。然而在 Node.js 这样的单线程环境下进行耗时长的同步计算不是个好主意，我们建议尽可能地使您的 loader 异步化。但如果计算量很小，同步 loader 也是可以的。

### "Raw" loader

默认情况下，资源文件会被转化为 UTF-8 字符串，然后传给 loader。通过设置`raw`，loader 可以接收原始的`Buffer`。每一个 loader 都可以用`String`或者`Buffer`的形式传递它的处理结果。编译器将会把它们在 loader 之间相互转换。

**raw-loader.js**

```js
module.exports = function(content) {
    assert(content instanceof Buffer);
    return someSyncOperation(content);
    // 返回值也可以是一个 `Buffer`
    // 即使不是 raw loader 也没问题
};
module.exports.raw = true;
```

### Pitching loader

loader**总是**从右到左地被调用，但是在一些情况下，loader 不需要关心之前处理的结果或者资源\(resource\)，而是只关心**元数据\(metadata\)**。在 loader 被调用前（从右到左），loader 中的`pitch`方法会**从左到右**依次被调用。

如果中间某个 loader 的`pitch`方法返回了一个值，那么剩下的 loader 都会被跳过，转而从当前 loader 开始向左调用 loader。`data`可以在 pitch 和普通的 loader 调用间传递。

```js
module.exports = function(content) {
    return someSyncOperation(content, this.data.value);
};
module.exports.pitch = function(remainingRequest, precedingRequest, data) {
    if(someCondition()) {
        // 直接返回
        return "module.exports = require(" + JSON.stringify("-!" + remainingRequest) + ");";
    }
    data.value = 42;
};
```

## The loader context

loader context 表示在 loader 内使用`this`可以访问的一些方法或属性

假设我们在`/abc/file.js`中这样请求加载别的模块：

```js
require("./loader1?xyz!loader2!./resource?rrr");
```

### `this.version`

**loader API 的版本号。**目前是`2`。这对于向后兼容性有一些用处。通过这个版本号，你可以为不同版本间的破坏性变更编写不同的逻辑，或做降级处理。

### `this.context`

**模块所在的目录。**某些场景下这可能会有用处。

在我们的例子中：这个属性为`/abc`，因为`resource.js`在这个目录中

### `this.request`

被解析出来的请求字符串。

在我们的例子中：`"/abc/loader1.js?xyz!/abc/node_modules/loader2/index.js!/abc/resource.js?rrr"`

### `this.query`

1. 如果这个 loader 配置了[`options`](https://doc.webpack-china.org/configuration/module/#useentry)对象的话，`this.query`就指向这个 option 对象。
2. 如果 loader 中没有`options`，而是以 query 字符串作为参数调用时，`this.query`就是一个以`?`开头的字符串。

> **\[warning\]** 注：
> `options`已取代`query`，所以此属性废弃。使用`loader-utils`中的[`getOptions`方法](https://github.com/webpack/loader-utils#getoptions)来提取给定 loader 的 option。

  


