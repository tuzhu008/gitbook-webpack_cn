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

```js
require(dependency: String)
```

同步从另一个模块检索导出。编译器将确保依赖项在输出bundle中可用。

```js
var $ = require("jquery");
var myModule = require("my-module");
```

> **\[warning\]**注：
>
> 异步地使用它可能没有预期的效果。

### `require.resolve`

```js
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

```js
var d1 = require("dependency");
require("dependency") === d1
delete require.cache[require.resolve("dependency")];
require("dependency") !== d1
```

```js
// in file.js
require.cache[module.id] === module
require("./file.js") === module.exports
delete require.cache[module.id];
require.cache[module.id] === undefined
require("./file.js") !== module.exports // in theory; in praxis this causes a stack overflow
require.cache[module.id] !== module
```

### `require.ensure`

W&gt; `require.ensure()` is specific to webpack and s by `import()`.

> **\[warning\]**注：
>
> `require.ensure()`是特定于webpack的，并被`import()`取代

```javascript
require.ensure(dependencies: String[], callback: function(require), errorCallback: function(error), chunkName: String)
```

将给定的`dependencies`拆分为一个单独的bundle，该bundle将被异步加载。在使用CommonJS模块语法时，这是动态加载依赖项的唯一方法。也就是说，这段代码可以在**执行上下文（**execution**）**中运行，只有在满足特定条件时才加载`dependencies`。

W&gt; This feature relies on [`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) internally. If you use `require.ensure` with older browsers, remember to shim `Promise` using a polyfill such as [es6-promise](https://github.com/stefanpenner/es6-promise) or [promise-polyfill](https://github.com/taylorhakes/promise-polyfill).

> **\[warning\]**注：
>
> 这个特性在内部依赖于[`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)。如果你在老旧浏览器中使用`require.ensure`，记得添加polyfill。如[es6-promise](#)或者[promise-polyfill](#)

```js
var a = require('normal-dep');

if ( module.hot ) {
  require.ensure(['b'], function(require) {
    var c = require('c');

    // Do something special...
  });
}
```

以下参数按照上面指定的顺序来阐述:

* `dependencies`:  字符串数组，声明要执行的`callback`中的代码所需要的所有模块。
* `callback`:  回调函数，一旦加载了依赖项，webpack将会执行它。`require`函数的实现被作为参数发送给这个函数。函数体可以使用它来进一步`require()`它需要执行的模块。
* `errorCallback`:  当webpack无法加载依赖项时执行的函数。
* `chunkName`:  这个特定的`require.ensure()`所创建的chunk的名称。通过将相同的`chunkName`传递给各种`require.ensure()`调用，我们可以将它们的代码合并到一个单独的chunk中，只产生一个浏览器必须加载的bundle。

> **\[warning\]**注：
>
> 尽管`require`的实现被作为一个参数传递给`callback`函数，在`callback`中可以使用一个任意的名称，不是由webpack的静态解析器处理的，例如`equire.ensure([], function(request) { request('someModule'); })。`所以请使用`require`代替，例如`require.ensure([], function(require) { require('someModule'); })`。

## AMD

异步模块定义\(AMD\)是一个JavaScript规范，它定义了一个用于编写和加载模块的接口。以下的AMD方法是被webpack支持的:

### `define` \(使用工厂函数\)

```js
define([name: String], [dependencies: String[]], factoryMethod: function(...))
```

如果提供了`dependencies`，那么将使用每个依赖项的导出\(按照相同的顺序\)调用`factoryMethod`。如果不提供`dependencies`，则会使用`require`、`exports`和`module`\(为了兼容性！\)调用`factoryMethod`。如果该函数返回一个值，则该值将由模块导出。编译器确保每个依赖项都可用

> **\[warning\]**注：
>
> webpack忽略了`name`参数。

```js
define(['jquery', 'my-module'], function($, myModule) {
  // Do something with $ and myModule...

  // Export a function
  return function doSomething() {
    // ...
  };
});
```

> **\[warning\]**注：
>
> 这**不能**在异步函数中使用。

### `define` \(使用 value\)

```js
define(value: !Function)
```

这将简单地导出所提供的`value`。这里的`value`可以是任何东西，只要**不是**一个函数。

```js
define({
  answer: 42
});
```

> **\[warning\]**注
>
> 这**不能**在异步函数中使用。

### `require` \(amd-version\)

```js
require(dependencies: String[], [callback: function(...)])
```

类似于`require.ensure`，这将把给定的`dependencies`划分为一个单独的bundle，它将被**异步**加载。`callback`将在`dependencies`数组中对每个依赖项的导出进行调用。

> **\[warning\] **注：
>
> 这个特性在内部依赖于[`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)。如果你在老旧浏览器\(例如 Internet Explorer 11\)中使用AMD，记得添加polyfill。如[es6-promise](#)或者[promise-polyfill](#)

```js
require(['b'], function(b) {
  var c = require("c");
});
```

> **\[warning\] **注:
>
> 没有提供指定chunk名称的选项。

## Labeled Modules

内部的`LabeledModulesPlugin`使您可以使用以下方法来导出和请求您的模块:

### `export` 标签

导出给定的`value`。这个标签可以在函数声明或变量声明之前出现。函数名或变量名是输出值的标识符。

```js
export: var answer = 42;
export: function method(value) {
  // Do something...
};
```

W&gt; Using it in an async function may not have the expected effect.

> **\[warning\] **注:
>
> 在异步函数中使用它可能没有预期的效果。

### `require` 标签

使当前范围内的依赖项中所有导出可用。`require`标签可以出现在字符串之前。依赖项必须导出带有`export`标签的值。不能使用CommonJS或AMD模块。

**some-dependency.js**

```js
export: var answer = 42;
export: function method(value) {
  // Do something...
};
```

```js
require: 'some-dependency';// 依赖文件名称
console.log(answer);
method(...);
```

## Webpack

除了上面提到的模块语法之外，webpack还允许一些自定义的、webpack特有的方法:

### `require.context`

```js
require.context(directory:String, includeSubdirs:Boolean /* 可选, 默认值 true */, filter:RegExp /* 可选 */)
```

使用`directory`的路径指定一组依赖项，一个`includeSubdirs`选项，以及一个对包含的模块进行更细粒度控制的`filter（过滤器）`。之后，这些问题就可以轻松解决了:

```js
var context = require.context('components', true, /\.html$/);
var componentA = context.resolve('componentA');
```

### `require.include`

```javascript
require.include(dependency: String)
```

包含一个`dependency`而不执行它。这可以用于在输出chunk中优化模块的位置。

```js
require.include('a');
require.ensure(['a', 'b'], function(require) { /* ... */ });
require.ensure(['a', 'c'], function(require) { /* ... */ });
```

这将导致以下输出:

* entry chunk: `file.js` and `a`
* 匿名（anonymous）chunk: `b`
* 匿名（anonymous）chunk: `c`

如果没有`require.include('a')`，它将被复制进匿名chunk中。

### `require.resolveWeak`

类似于`require.resolve`，但这不会将`module`拉入bundle中。这就是所谓的“弱\(weak\)”依赖性。

```javascript
if(__webpack_modules__[require.resolveWeak('module')]) {
  // 当模块可用时做一些事情...
}
if(require.cache[require.resolveWeak('module')]) {
  // 在模块加载之前做一些事情...
}

// 你可以执行动态 resolves ("context")，就像其他的require/import方法一样。
const page = 'Foo';
__webpack_modules__[require.resolveWeak(`./page/${page}`)]
```

> **\[info\]** 注：
>
> `require.resolveWeak`是通用渲染\(SSR + 代码分割\)的基础\(SSR+代码分裂\)，就像在诸如[react-通用组件](https://github.com/faceyspacey/react-universal-component)这样的包中使用。它允许代码在客户机上同步地渲染服务器和初始页面加载。它要求chunk是被手动提供，或者是某种可用的。它能够请求（require）模块，而不需要指出它们应该被打包到一个chunk中。它与`import()`结合使用，当用户导航触发其他导入时，它将接管该操作。



