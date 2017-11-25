[![npm][npm]][npm-url]
[![deps][deps]][deps-url]
[![test][test]][test-url]
[![chat][chat]][chat-url]

<div align="center">
  <!-- replace with accurate logo e.g from https://worldvectorlogo.com/ -->
  <a href="https://github.com/webpack/webpack">
    <img width="200" height="200" vspace="" hspace="25"
      src="https://webpack.js.org/assets/icon-square-big.svg">
  </a>
  <h1>Imports Loader</h1>
  <p>imports loader允许您使用依赖于特定全局变量的模块。<p>
  <a href="https://github.com/webpack-contrib/imports-loader"> <img src="https://img.shields.io/badge/Github-%E6%9F%A5%E7%9C%8B%E6%9B%B4%E5%A4%9A-brightgreen.svg"></a>
</div>

这对于依赖全局变量的第三方模块非常有用，比如`$`或者`this`是`window`对象。imports loader可以添加必要的`require('whatever')`调用，所以这些模块和webpack一起工作。

<h2 align="center">安装</h2>

```bash
npm install imports-loader -save-dev
```

<h2 align="center"><a href="https://webpack.js.org/concepts/loaders">用法</a></h2>

有了这个文件 `example.js`

```javascript
$("img").doSomeAwesomeJqueryPluginStuff();
```

然后，您可以通过配置 imports-loader 来将`$`变量注入到模块中:

``` javascript
require("imports-loader?$=jquery!./example.js");
```

这就简单地把 `var $ = require("jquery");` 前置插入到`example.js`.

### 语法

查询值 | 等于
------------|-------
`angular` | `var angular = require("angular");`
`$=jquery` | `var $ = require("jquery");`
`define=>false` | `var define = false;`
`config=>{size:50}` | `var config = {size:50};`
`this=>window` | `(function () { ... }).call(window);`

### 多个值

多个值用逗号分隔`,`:

```javascript
require("imports-loader?$=jquery,angular,config=>{size:50}!./file.js");
```

### webpack.config.js

和往常一样，在`webpack.config.js`中配置更好。

```javascript
// ./webpack.config.js

module.exports = {
    ...
    module: {
        rules: [
            {
                test: require.resolve("some-module"),
                use: "imports-loader?this=>window"
            }
        ]
    }
};
```

<h2 align="center">典型用例</h2>

### jQuery插件

`imports-loader?$=jquery`

### 自定的 Angular 模块

`imports-loader?angular`

### 禁用 AMD

有很多模块在使用 CommonJS 前会进行 `define` 函数的检查。自从 webpack 两种格式都可以使用后，在这种场景下默认使用了 AMD 可能会造成某些问题（如果接口的实现比较古怪）。

你可以像下面这样轻松的禁用 AMD

```javascript
imports-loader?define=>false
```

关于兼容性问题的更多提示，可以参考官方的文档 [Shimming Modules](http://webpack.github.io/docs/shimming-modules.html)。

<h2 align="center">维护者</h2>

<table>
  <tbody>
    <tr>
      <td align="center">
        <img width="150" height="150"
        src="https://avatars3.githubusercontent.com/u/166921?v=3&s=150">
        </br>
        <a href="https://github.com/bebraw">Juho Vepsäläinen</a>
      </td>
      <td align="center">
        <img width="150" height="150"
        src="https://avatars2.githubusercontent.com/u/8420490?v=3&s=150">
        </br>
        <a href="https://github.com/d3viant0ne">Joshua Wiens</a>
      </td>
      <td align="center">
        <img width="150" height="150"
        src="https://avatars3.githubusercontent.com/u/533616?v=3&s=150">
        </br>
        <a href="https://github.com/SpaceK33z">Kees Kluskens</a>
      </td>
      <td align="center">
        <img width="150" height="150"
        src="https://avatars3.githubusercontent.com/u/3408176?v=3&s=150">
        </br>
        <a href="https://github.com/TheLarkInn">Sean Larkin</a>
      </td>
    </tr>
  <tbody>
</table>


[npm]: https://img.shields.io/npm/v/imports-loader.svg
[npm-url]: https://npmjs.com/package/imports-loader

[deps]: https://david-dm.org/webpack-contrib/imports-loader.svg
[deps-url]: https://david-dm.org/webpack-contrib/imports-loader

[chat]: https://img.shields.io/badge/gitter-webpack%2Fwebpack-brightgreen.svg
[chat-url]: https://gitter.im/webpack/webpack

[test]: http://img.shields.io/travis/webpack-contrib/imports-loader.svg
[test-url]: https://travis-ci.org/webpack-contrib/imports-loader
