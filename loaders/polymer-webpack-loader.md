# polymer-webpack-loader
[![](https://img.shields.io/badge/Github-查看更多-brightgreen.svg)](https://github.com/webpack-contrib/polymer-webpack-loader)
[![npm version](https://badge.fury.io/js/polymer-webpack-loader.svg)](https://badge.fury.io/js/polymer-webpack-loader)
[![build status](https://travis-ci.org/webpack-contrib/polymer-webpack-loader.svg?branch=master)](https://travis-ci.org/webpack-contrib/polymer-webpack-loader)

> [Polymer](https://www.polymer-project.org/) component loader for [webpack](https://webpack.js.org/).

此loader允许你编写混合的HTML，CSS和JavaScript聚合物元素，并且仍然使用完整的webpack生态系统，包括模块打包和代码分割。


<img width="1024" alt="" src="https://user-images.githubusercontent.com/1066253/28131928-3b257288-66f0-11e7-8295-cb968cefb040.png">

此加载器转换你的模块:

 * `<link rel="import" href="./my-other-element.html">` -> `import './my-other-element.html';`
 * `<dom-module>` 变为在运行时注册的字符串
 * `<script src="./other-script.js"></script>` -> `import './other-script.js';`
 * `<script>/* contents */</script>` -> `/* contents */`

这是什么意思?
 
 任何**不是一个外部连接** 并且不以```~```, ```/```, ```./``` 或者一连串的 ```../``` 开头的 ```<link>``` "href" 或者 ```<script>``` "src" ，```./``` 都会被附加到它们的值的开始。 为了防止这种变化，请使用下面的选项。 

## 路径翻译

| `tag`                            | `import`                        |
| ----------------------------------- | ------------------------------------- |
| `<link rel="import" href="path/to/some-element.html">`     | `import "./path/to/some-element.html"` 【译者注：如上面描述的，`./`被添加到路径前面】 |
| `<link rel="import" href="/path/to/some-element.html">`    | `import "/path/to/some-element.html"`   |
| `<link rel="import" href="../path/to/some-element.html">`  | `import "../path/to/some-element.html"` |
| `<link rel="import" href="./path/to/some-element.html">`   | `import "./path/to/some-element.html"`  |
| `<link rel="import" href="~path/to/some-element.html">`    | `import "~path/to/some-element.html"`   |

## 配置loader

```javascript
{
  test: /\.html$/,
  include: Condition(s) (optional),
  exclude: Condition(s) (optional),
  options: {
    ignoreLinks: Condition(s) (optional),
    ignorePathReWrite: Condition(s) (optional),
    processStyleLinks: Boolean (optional),
    htmlLoader: Object (optional)
  },
  loader: 'polymer-webpack-loader'
},
```

### include: 条件(s)

参见 webpack文档中的 [Rule.include] and [Condition]。 匹配此选项的路径将由polymer-webpack-loader处理。  
> **[warning]** 警告：
>
> 如果这个属性存在，loader只会处理匹配给定条件的文件。如果你的组件有一个指向一个组件的`<link>`，例如，另一个目录，`include`条件也**必须**匹配那个目录。

[Rule.include]: //configuration/module.md#ruleinclude
[Condition]: //configuration/module.md#rule条件

### exclude: 条件(s)

参见 webpack 文档中的 [Rule.exclude] 和 [Condition] 。匹配此选项的路径将不会被polymer-webpack-loader处理。  
> **[info]** 注：
> 
> 通过`<link>`导入的文件将不会被该属性排除。参见`Options.ignoreLinks`。

[Rule.exclude]: //configuration/module.md#ruleexclude

### 选项

#### ignoreLinks: 条件

`<link>`指向匹配这些条件的路径将不会被转换为`import` (参见webpack中的[Condition]) 。

#### ignorePathReWrite: 条件

当转换为`import`时，匹配这些条件(参见webpack中的[Condition])的`<link>`路径将不会被更改。这对于别名、loader语法(例如:`markup-inline-loader!./my-element.html`)或模块路径而言非常有用。

#### processStyleLinks Boolean

如果设置为`true`，loader将使用`<style>require('...')</style>`在 dom-module 内部重写`<link import="css" href="...">`或者`<link rel="stylesheet" href="...">`。这将允许在webpack config中设置的被loader处理的文件，以处理它们的文件类型。

1. 任何在 `<dom-module>` 中但不在 `<template>` 中的 `<link>` 将按照它们在文件中标签的顺序被添加到 `<template>`。

  ```html
    <dom-module>
      <link rel="stylesheet" href="file1.css">
      <template>
        <link rel="stylesheet" href="file2.css">
      </template>
    </dom-module>
  
    将生成：
  
    <dom-module>
      <template>
        <style>require('file1.css')</style>
        <style>require('file2.css')</style>
      </template>
    </dom-module>
  ```

2. 如果href是一个相对路径，此loader只会替换一个`<link>`。任何试图访问外部连接的`<link>`。例如 `http`, `https` 或者 `//` 将不会被替换。

#### htmlLoader: Object

传递给html-loader的选项。 参见 [html-loader](/loaders/html-loader.md)

### 与 Babel一起使用 (或者其他JS 编译器)

如果你想要对元素的`<script>`块内容进行编译，你可以[串联一个额外的loader](//configuration/module.mdl#ruleuse)。

```js
module: {
  loaders: [
    {
      test: /\.html$/,
      use: [
        // 链式loader 从下到上被应用 
        { loader: 'babel-loader' },
        { loader: 'polymer-webpack-loader' }
      ]
    }
  ]
}

// 替代语法

module: {
  loaders: [
    {
      test: /\.html$/,
      // 链式loader 从右到左被应用 
      loader: 'babel-loader!polymer-webpack-loader'
    }
  ]
}
```

###  HtmlWebpackPlugin的使用

您可能会遇到与olymer-webpack-loader的冲突，这取决于您如何配置HtmlWebpackPlugin，。

Example: 

```javascript
{
  test: /\.html$/,
  loader: 'html-loader',
  include: [
    path.resolve(__dirname, './index.html'),
  ],
},
{
  test: /\.html$/,  
  loader: 'polymer-webpack-loader'
}
```

这将基于被 html-loader 使用的过程将 index.html 文件暴露给 polymer-webpack-loader 。在这种情况下，您需要将 html 文件从 polymer-webpack-loader 中排除，或者寻找其他方法来避免这种冲突。参见: [html-webpack-plugin template options](//Plugins/third-party/HtmlWebpackPlugin.md#模板选项)


## Shimming

并不是所有的 Polymer 元素都被编写成一个模块，并且需要对webpack进行修改。遇到的最常见的问题是模块不在全局作用域内执行。变量、函数和类将不再是全局的，除非它们在全局对象(window)中被声明为属性。

```js
class MyElement {} // 我不是全局变凉了
window.myElement = MyElement; // 现在我又是全局变量了
```

对于外部代码库代码, webpack 提供 [shimming 选项](https://webpack.js.org/guides/shimming/).

 * 使用 [exports-loader](//loaders/exports-loader.md) 来向组件添加模块的导出，这些组件期望符号（symbol）是全局的。
   
 * 使用 i[mports-loader](//loaders/imports-loader.md)，当脚本期望`this`关键字引用`window`时。
 
 * 使用 [ProvidePlugin](https://webpack.js.org/guides/shimming/#provideplugin) 来添加一个模块 import 语句，当脚本期望变量被全局定义时 (但现在为模块的导出)。
 
 * 使用 [NormalModuleReplacementPlugin](/Plugins/internal/NormalModuleReplacementPlugin.md) 来让webpack调换一个用于脚本的模块兼容版本。
   
您可能需要将多个shimming技术应用于相同的组件。

## 引导应用程序

web组件 polyfills 必须以特定的顺序添加。如果不延迟加载主bundle和组件，可以在浏览器控制台中看到以下的异常:

```js
Uncaught TypeError: Failed to construct 'HTMLElement': Please use the 'new' operator, this DOM object constructor cannot be called as a function.
// 未捕获类型错误:未能构造“HTMLElement”:请使用“new”操作符，这个DOM对象构造函数不能被作为函数调用。
```

引用[demo html file](https://github.com/webpack-contrib/polymer-webpack-loader/blob/master/demo/src/index.ejs)
来查看适当的加载顺序。

<h2 align="center">维护者</h2>

<table>
  <tbody>
    <tr>
      <td align="center">
        <a href="https://github.com/bryandcoulter">
          <img width="150" height="150" src="https://avatars.githubusercontent.com/u/18359726?v=3">
          </br>
          Bryan Coulter
        </a>
      </td>
      <td align="center">
        <a href="https://github.com/ChadKillingsworth">
          <img width="150" height="150" src="https://avatars.githubusercontent.com/u/1247639?v=3">
          </br>
          Chad Killingsworth
        </a>
      </td>
      <td align="center">
        <a href="https://github.com/robdodson">
          <img width="150" height="150" src="https://avatars.githubusercontent.com/u/1066253?v=3">
          </br>
          Rob Dodson
        </a>
      </td>
    </tr>
  <tbody>
</table>
