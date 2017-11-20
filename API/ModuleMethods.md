# Module Methods \(模块方法\)

本节涵盖了在使用webpack编译代码中可用的所有方法。当使用webpack来打包应用程序时，您可以从各种模块语法风格中选择，包括[ES6](https://en.wikipedia.org/wiki/ECMAScript#6th_Edition_-_ECMAScript_2015), [CommonJS](https://en.wikipedia.org/wiki/CommonJS), 和 [AMD](https://en.wikipedia.org/wiki/Asynchronous_module_definition).

> **\[warning\]** 注：
>
> 虽然webpack支持多种模块语法，但我们建议使用单一语法来保持一致性，避免奇怪的行为/bug。这里有一个混合ES6和CommonJS的[ 例子](https://github.com/webpack/webpack.js.org/issues/552)，当然也有其他的例子。

## ES6 \(推荐\)

webpack的版本2支持ES6模块语法，这意味着您可以使用`import` 和 `export`，而不需要像babel 这样的工具来处理这个问题。请记住，您仍然可能需要其他ES6+特性的babel。以下方法被webpack支持:

### `import`

静态 `import` 其他模块的`export`

```js
import MyModule from './my-module.js';
import { NamedExport } from './other-module.js';
```

> **\[warning\]** 注：
>
> 这里的关键字是**静态**。正常的`import`语句不能在其他逻辑中动态地使用，也不能包含变量。请参阅下面的[规范](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)，了解更多信息和`import()`动态用法。

### `export`

导出任何作为`default`或者命名的导出

```js
// 命名 exports
export var Count = 5;
export function Multiply(a, b) {
  return a * b;
}

// 默认 export
export default {
  // Some data...
}
```

### `import()`

`import('path/to/module') -> Promise`

动态加载模块。对`import()`的调用被视为分割点，这意味着被请求的模块和它的子节点（children）被分割成一个单独的chunk。

T&gt; The [ES2015 Loader spec](https://whatwg.github.io/loader/) defines `import()` as method to load ES2015 modules dynamically on runtime.

> **\[info\]** 注：
>
> [ES2015 Loader 规范](https://whatwg.github.io/loader/) 定义了`import()`作为方法来在运行时动态加载 ES2015 模块

```js
if ( module.hot ) {
  import('lodash').then(_ => {
    // Do something with lodash (a.k.a '_')...
  })
}
```

> **\[warning\]** 注：
>
> 这个特性在内部依赖于[`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)。如果你在老旧浏览器中使用`import()`，记住使用一个pliyfill来塞住（shim）`Promise`。例如使用[es6-promise](#)或者[promise-polyfill](#)

`import`规范不允许不允许对chunk的名称或其他属性进行控制，因为“chunk”只是webpack中的一个概念。幸运的是，webpack通过注释允许一些特殊的参数，这样就不会违反规范:

```js
import(
  /* webpackChunkName: "my-chunk-name" */
  /* webpackMode: "lazy" */
  'module'
);
```

`webpackChunkName`:  新chunk的名称。从webpack 2.6.0起，在给定的字符串中，占位符`[index]`和`[request]`分别被支持为一个递增的数字或实际已解析的文件名。

`webpackMode`: 从 webpack 2.6.0起，可以指定用于解析动态导入的不同模式。以下选项是支持的:

* `"lazy"` \(默认\): 为每一个被`import()`的模块生成一个懒加载chunk
* `"lazy-once"`: 生成一个单一的懒加载chunk，可以满足`import`的所有调用。在第一次调用`import()`时将获取chunk，随后对`import()`的调用将使用相同的网络响应。请注意，这只在部分动态语句的情况下才有意义，例如``import(`./locales/${language}.json`)``，其中有多个模块路径可能会被请求。
* `"eager"`:  不生成额外的chunk。所有的模块都包含在当前的块中，不需要额外的网络请求。`Promise`仍会被返回，但已得到解决。与静态导入不同的是，在调用`import()`之前，模块不会被执行。
* `"weak"`: 如果模块函数已经以其他方式加载，则尝试装入模块\(例如，导入它的另一个chunk或包含加载了模块的脚本\)。仍然会返回一个`Promise`，但是，只有当chunk已经在客户端上才能成功地解析。如果模块不可用，则`Promise`被拒绝。网络请求永远不会被执行。当需要的chunk总是在初始请求\(嵌入在页面内\)中手动提供时，这对于通用的渲染是很有用的，但是在应用程序导航将触发导入而不是初始请求的情况下，这是很有用的。

> **\[info\]** 注：
>
> 这两个选项都可以像`/* webpackMode: "lazy-once", webpackChunkName: "all-i18n-data" */`那样组合在一起。它被解析为一个没有花括号（`{}`）的JSON5对象

-

> **\[warning\]**注：
>
> 完全动态的语句**将会失败**，例如`import(foo)`，因为webpack至少需要一些文件位置信息。这是因为`foo`可能是您的系统或项目中的任何文件的任何路径。`import()`必须至少包含关于模块所在位置的一些信息，因此bundling可以被限制为特定的目录或文件集。

-

> **\[warning\]**注：
>
> 包含在`import()`调用中可能被请求的每个模块都包括在内。例如，``import(`./locale/${language}.json`)``将导致`./locale`目录中的每个`.json`文件被打包到到新chunk中。在运行时，当变量`language`被计算时，任何文件如`english.json` 或 `german.json`将可以用于打包。

-

> **\[warning\]**注：
>
> 在webpack中使用`System.import`[不符合推荐的规范](https://github.com/webpack/webpack/issues/2163)，因此在webpack 2.1.0测试版中被弃用，转而支持`import()`。

## CommonJS

CommonJS的目标是在浏览器之外为JavaScript指定一个生态系统。下面的CommonJS方法是由webpack支持的:

### `require`

```javascript
require(dependency: String)
```

同步从另一个模块检索导出。编译器将确保依赖项在输出bundle中可用。

```javascript
var $ = require("jquery");
var myModule = require("my-module");
```

> **\[warning\]**注：
>
> 异步地使用它可能没有预期的效果。

### `require.resolve`

```javascript
require.resolve(dependency: String)
```

同步检索模块的ID。编译器将确保依赖项在输出bundle中可用。查看[`module.id`](/api/module-variables#module-id-commonjs-)以更多信息。

> **\[warning\]**注：
>
> 模块ID是webpack中的一个数字\(与NodeJS相比，它是一个字符串——文件名\)。

### `require.cache`

类型：数组

同一模块的多次require只会有一次模块执行和一次导出。因此在运行时存在一个缓存。从这个缓存中删除值会导致新的模块执行和新的导出。

> **\[warning\]**注：
>
> 这种情况只需要在少数情况下进行兼容性！

```javascript
var d1 = require("dependency");
require("dependency") === d1
delete require.cache[require.resolve("dependency")];
require("dependency") !== d1
```

```javascript
// in file.js
require.cache[module.id] === module
require("./file.js") === module.exports
delete require.cache[module.id];
require.cache[module.id] === undefined
require("./file.js") !== module.exports // in theory; in praxis this causes a stack overflow
require.cache[module.id] !== module
```

### `require.ensure`

W&gt; `require.ensure()` is specific to webpack and superseded by `import()`.

```javascript
require.ensure(dependencies: String[], callback: function(require), errorCallback: function(error), chunkName: String)
```

Split out the given `dependencies` to a separate bundle that that will be loaded asynchronously. When using CommonJS module syntax, this is the only way to dynamically load dependencies. Meaning, this code can be run within execution, only loading the `dependencies` if certain conditions are met.

W&gt; This feature relies on [`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) internally. If you use `require.ensure` with older browsers, remember to shim `Promise` using a polyfill such as [es6-promise](https://github.com/stefanpenner/es6-promise) or [promise-polyfill](https://github.com/taylorhakes/promise-polyfill).

```javascript
var a = require('normal-dep');

if ( module.hot ) {
  require.ensure(['b'], function(require) {
    var c = require('c');

    // Do something special...
  });
}
```

The following parameters are supported in the order specified above:

* `dependencies`: An array of strings declaring all modules required for the code in the `callback` to execute.
* `callback`: A function that webpack will execute once the dependencies are loaded. An implementation of the `require` function is sent as a parameter to this function. The function body can use this to further `require()` modules it needs for execution.
* `errorCallback`: A function that is executed when webpack fails to load the dependencies.
* `chunkName`: A name given to the chunk created by this particular `require.ensure()`. By passing the same `chunkName` to various `require.ensure()` calls, we can combine their code into a single chunk, resulting in only one bundle that the browser must load.

W&gt; Although the implementation of `require` is passed as an argument to the `callback` function, using an arbitrary name e.g. `require.ensure([], function(request) { request('someModule'); })` isn't handled by webpack's static parser. Use `require` instead, e.g. `require.ensure([], function(require) { require('someModule'); })`.

## AMD

Asynchronous Module Definition \(AMD\) is a JavaScript specification that defines an interface for writing and loading modules. The following AMD methods are supported by webpack:

### `define` \(with factory\)

```javascript
define([name: String], [dependencies: String[]], factoryMethod: function(...))
```

If `dependencies` are provided, `factoryMethod` will be called with the exports of each dependency \(in the same order\). If `dependencies` are not provided, `factoryMethod` is called with `require`, `exports` and `module` \(for compatibility!\). If this function returns a value, this value is exported by the module. The compiler ensures that each dependency is available.

W&gt; Note that webpack ignores the `name` argument.

```javascript
define(['jquery', 'my-module'], function($, myModule) {
  // Do something with $ and myModule...

  // Export a function
  return function doSomething() {
    // ...
  };
});
```

W&gt; This CANNOT be used in an asynchronous function.

### `define` \(with value\)

```javascript
define(value: !Function)
```

This will simply export the provided `value`. The `value` here can be anything except a function.

```javascript
define({
  answer: 42
});
```

W&gt; This CANNOT be used in an async function.

### `require` \(amd-version\)

```javascript
require(dependencies: String[], [callback: function(...)])
```

Similar to `require.ensure`, this will split the given `dependencies` into a separate bundle that will be loaded asynchronously. The `callback` will be called with the exports of each dependency in the `dependencies` array.

W&gt; This feature relies on [`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) internally. If you use AMD with older browsers \(e.g. Internet Explorer 11\), remember to shim `Promise` using a polyfill such as [es6-promise](https://github.com/stefanpenner/es6-promise) or [promise-polyfill](https://github.com/taylorhakes/promise-polyfill).

```javascript
require(['b'], function(b) {
  var c = require("c");
});
```

W&gt; There is no option to provide a chunk name.

## Labeled Modules

The internal `LabeledModulesPlugin` enables you to use the following methods for exporting and requiring within your modules:

### `export` label

Export the given `value`. The label can occur before a function declaration or a variable declaration. The function name or variable name is the identifier under which the value is exported.

```javascript
export: var answer = 42;
export: function method(value) {
  // Do something...
};
```

W&gt; Using it in an async function may not have the expected effect.

### `require` label

Make all exports from the dependency available in the current scope. The `require` label can occur before a string. The dependency must export values with the `export` label. CommonJS or AMD modules cannot be consumed.

**some-dependency.js**

```javascript
export: var answer = 42;
export: function method(value) {
  // Do something...
};
```

```javascript
require: 'some-dependency';
console.log(answer);
method(...);
```

## Webpack

Aside from the module syntaxes described above, webpack also allows a few custom, webpack-specific methods:

### `require.context`

```javascript
require.context(directory:String, includeSubdirs:Boolean /* optional, default true */, filter:RegExp /* optional */)
```

Specify a whole group of dependencies using a path to the `directory`, an option to `includeSubdirs`, and a `filter` for more fine grained control of the modules included. These can then be easily resolved later on:

```javascript
var context = require.context('components', true, /\.html$/);
var componentA = context.resolve('componentA');
```

### `require.include`

```javascript
require.include(dependency: String)
```

Include a `dependency` without executing it. This can be used for optimizing the position of a module in the output chunks.

```javascript
require.include('a');
require.ensure(['a', 'b'], function(require) { /* ... */ });
require.ensure(['a', 'c'], function(require) { /* ... */ });
```

This will result in following output:

* entry chunk: `file.js` and `a`
* anonymous chunk: `b`
* anonymous chunk: `c`

Without `require.include('a')` it would be duplicated in both anonymous chunks.

### `require.resolveWeak`

Similar to `require.resolve`, but this won't pull the `module` into the bundle. It's what is considered a "weak" dependency.

```javascript
if(__webpack_modules__[require.resolveWeak('module')]) {
  // Do something when module is available...
}
if(require.cache[require.resolveWeak('module')]) {
  // Do something when module was loaded before...
}

// You can perform dynamic resolves ("context")
// just as with other require/import methods.
const page = 'Foo';
__webpack_modules__[require.resolveWeak(`./page/${page}`)]
```

T&gt; `require.resolveWeak` is the foundation of _universal rendering_ \(SSR + Code Splitting\), as used in packages such as [react-universal-component](https://github.com/faceyspacey/react-universal-component). It allows code to render synchronously on both the server and initial page-loads on the client. It requires that chunks are manually served or somehow available. It's able to require modules without indicating they should be bundled into a chunk. It's used in conjunction with `import()` which takes over when user navigation triggers additional imports.

---

> 原文：[https://webpack.js.org/api/module-methods/](https://webpack.js.org/api/module-methods/)



