# I18nWebpackPlugin

[![](https://img.shields.io/badge/Github-查看更多-brightgreen.svg)](https://github.com/webpack-contrib/i18n-webpack-plugin)

将本地化嵌入到你的webpack中。

## 安装

```bash
npm i -D i18n-webpack-plugin
```

## 用法

此插件会创建包含译文的 bundle。所以您可以将翻译后的 bundle 提供给客户端。

```js
new I18nPlugin(languageConfig, optionsObj)
```

## 配置

### `languageConfig`

### `optionsObj`

类型：object

有以下选项：

* `functionName`：默认值`__`, 你可以更改为其他函数名。

* `failOnMissing`：默认值为`false`，找不到映射文本\(mapping text\)时会给出一个警告信息，如果设置为`true`，则会给出一个错误信息。

* `hideMessage`：默认值为`false`，将会显示警告/错误信息。如果设置为`true`，警告/错误信息将会被隐藏。

## 示例

这个例子使用了`I18nPlugin`和多编译器的特性。

`webpack.config.js`导出了应该被编译的所有配置组合的数组。在本例中，使用了两个不同参数`I18nPlugin`。

I18nPlugin用一个常量字符串替换了i18n函数`__(...)`的每一个事件。即。`__("Hello World")`和“Hello World”职责。“喂沿条”。例如`__("Hello World")`，替换后为`"Hallo Welt"`

### 例如：

```js
console.log(__("Hello World"));
console.log(__("Missing Text"));
```

### webpack.config.js

```js
var path = require("path");
var I18nPlugin = require("i18n-webpack-plugin");
var languages = {
    "en": null,
    "de": require("./de.json")
};
module.exports = Object.keys(languages).map(function(language) {
    return {
        name: language,
        entry: "./example",
        output: {
            path: path.join(__dirname, "js"),
            filename: language + ".output.js"
        },
        plugins: [
            new I18nPlugin(
                languages[language]
            )
        ]
    };
});
```

### de.json

```js
{
    "Hello World": "Hallo Welt"
}
```

### js/de.output.js

<!--sec data-title="Introduction" data-id="section0" data-show=false ces-->



```js
/******/ (function(modules) { // webpackBootstrap
/******/ 	// The module cache
/******/ 	var installedModules = {};
/******/
/******/ 	// The require function
/******/ 	function __webpack_require__(moduleId) {
/******/
/******/ 		// Check if module is in cache
/******/ 		if(installedModules[moduleId]) {
/******/ 			return installedModules[moduleId].exports;
/******/ 		}
/******/ 		// Create a new module (and put it into the cache)
/******/ 		var module = installedModules[moduleId] = {
/******/ 			i: moduleId,
/******/ 			l: false,
/******/ 			exports: {}
/******/ 		};
/******/
/******/ 		// Execute the module function
/******/ 		modules[moduleId].call(module.exports, module, module.exports, __webpack_require__);
/******/
/******/ 		// Flag the module as loaded
/******/ 		module.l = true;
/******/
/******/ 		// Return the exports of the module
/******/ 		return module.exports;
/******/ 	}
/******/
/******/
/******/ 	// expose the modules object (__webpack_modules__)
/******/ 	__webpack_require__.m = modules;
/******/
/******/ 	// expose the module cache
/******/ 	__webpack_require__.c = installedModules;
/******/
/******/ 	// define getter function for harmony exports
/******/ 	__webpack_require__.d = function(exports, name, getter) {
/******/ 		if(!__webpack_require__.o(exports, name)) {
/******/ 			Object.defineProperty(exports, name, {
/******/ 				configurable: false,
/******/ 				enumerable: true,
/******/ 				get: getter
/******/ 			});
/******/ 		}
/******/ 	};
/******/
/******/ 	// getDefaultExport function for compatibility with non-harmony modules
/******/ 	__webpack_require__.n = function(module) {
/******/ 		var getter = module && module.__esModule ?
/******/ 			function getDefault() { return module['default']; } :
/******/ 			function getModuleExports() { return module; };
/******/ 		__webpack_require__.d(getter, 'a', getter);
/******/ 		return getter;
/******/ 	};
/******/
/******/ 	// Object.prototype.hasOwnProperty.call
/******/ 	__webpack_require__.o = function(object, property) { return Object.prototype.hasOwnProperty.call(object, property); };
/******/
/******/ 	// __webpack_public_path__
/******/ 	__webpack_require__.p = "js/";
/******/
/******/ 	// Load entry module and return exports
/******/ 	return __webpack_require__(__webpack_require__.s = 0);
/******/ })
/************************************************************************/
```

<!--endsec-->



```js
/******/ ([
/* 0 */
/*!********************!*\
  !*** ./example.js ***!
  \********************/
/*! no static exports found */
/*! all exports used */
/***/ (function(module, exports, __webpack_require__) {

console.log("Hallo Welt");
console.log("Missing Text");

/***/ })
/******/ ]);

```
### js/en.output.js


```js
/******/ (function(modules) { // webpackBootstrap
/******/ 	// The module cache
/******/ 	var installedModules = {};
/******/
/******/ 	// The require function
/******/ 	function __webpack_require__(moduleId) {
/******/
/******/ 		// Check if module is in cache
/******/ 		if(installedModules[moduleId]) {
/******/ 			return installedModules[moduleId].exports;
/******/ 		}
/******/ 		// Create a new module (and put it into the cache)
/******/ 		var module = installedModules[moduleId] = {
/******/ 			i: moduleId,
/******/ 			l: false,
/******/ 			exports: {}
/******/ 		};
/******/
/******/ 		// Execute the module function
/******/ 		modules[moduleId].call(module.exports, module, module.exports, __webpack_require__);
/******/
/******/ 		// Flag the module as loaded
/******/ 		module.l = true;
/******/
/******/ 		// Return the exports of the module
/******/ 		return module.exports;
/******/ 	}
/******/
/******/
/******/ 	// expose the modules object (__webpack_modules__)
/******/ 	__webpack_require__.m = modules;
/******/
/******/ 	// expose the module cache
/******/ 	__webpack_require__.c = installedModules;
/******/
/******/ 	// define getter function for harmony exports
/******/ 	__webpack_require__.d = function(exports, name, getter) {
/******/ 		if(!__webpack_require__.o(exports, name)) {
/******/ 			Object.defineProperty(exports, name, {
/******/ 				configurable: false,
/******/ 				enumerable: true,
/******/ 				get: getter
/******/ 			});
/******/ 		}
/******/ 	};
/******/
/******/ 	// getDefaultExport function for compatibility with non-harmony modules
/******/ 	__webpack_require__.n = function(module) {
/******/ 		var getter = module && module.__esModule ?
/******/ 			function getDefault() { return module['default']; } :
/******/ 			function getModuleExports() { return module; };
/******/ 		__webpack_require__.d(getter, 'a', getter);
/******/ 		return getter;
/******/ 	};
/******/
/******/ 	// Object.prototype.hasOwnProperty.call
/******/ 	__webpack_require__.o = function(object, property) { return Object.prototype.hasOwnProperty.call(object, property); };
/******/
/******/ 	// __webpack_public_path__
/******/ 	__webpack_require__.p = "js/";
/******/
/******/ 	// Load entry module and return exports
/******/ 	return __webpack_require__(__webpack_require__.s = 0);
/******/ })
/************************************************************************/
/******/ ([
/* 0 */
/*!********************!*\
  !*** ./example.js ***!
  \********************/
/*! no static exports found */
/*! all exports used */
/***/ (function(module, exports, __webpack_require__) {

console.log("Hello World");
console.log("Missing Text");

/***/ })
/******/ ]);

```
### Info
#### Uncompressed


```bash
Hash: b61d16621736c97f557e52b4d8e68140f1345ef8
Version: webpack 3.5.1
Child en:
    Hash: b61d16621736c97f557e
           Asset     Size  Chunks             Chunk Names
    en.output.js  2.69 kB       0  [emitted]  main
    Entrypoint main = en.output.js
    chunk    {0} en.output.js (main) 65 bytes [entry] [rendered]
        > main [0] ./example.js 
        [0] ./example.js 65 bytes {0} [built]
Child de:
    Hash: 52b4d8e68140f1345ef8
           Asset     Size  Chunks             Chunk Names
    de.output.js  2.69 kB       0  [emitted]  main
    Entrypoint main = de.output.js
    chunk    {0} de.output.js (main) 65 bytes [entry] [rendered]
        > main [0] ./example.js 
        [0] ./example.js 65 bytes {0} [built] [1 warning]
    
    WARNING in ./example.js
    Missing localization: Missing Text

```
#### Minimized (uglify-js, no zip)


```bash
Hash: b61d16621736c97f557e52b4d8e68140f1345ef8
Version: webpack 3.5.1
Child en:
    Hash: b61d16621736c97f557e
           Asset       Size  Chunks             Chunk Names
    en.output.js  538 bytes       0  [emitted]  main
    Entrypoint main = en.output.js
    chunk    {0} en.output.js (main) 65 bytes [entry] [rendered]
        > main [0] ./example.js 
        [0] ./example.js 65 bytes {0} [built]
Child de:
    Hash: 52b4d8e68140f1345ef8
           Asset       Size  Chunks             Chunk Names
    de.output.js  537 bytes       0  [emitted]  main
    Entrypoint main = de.output.js
    chunk    {0} de.output.js (main) 65 bytes [entry] [rendered]
        > main [0] ./example.js 
        [0] ./example.js 65 bytes {0} [built] [1 warning]
    
    WARNING in ./example.js
    Missing localization: Missing Text

```





