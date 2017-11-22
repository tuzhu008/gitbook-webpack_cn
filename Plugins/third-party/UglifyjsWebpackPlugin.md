# UglifyjsWebpackPlugin

这个插件使用 [UglifyJS v3](https://github.com/mishoo/UglifyJS2/tree/harmony)[\(`uglify-es`\)](https://npmjs.com/package/uglify-es) 来压缩你的JavaScript。

> **\[info\]**
>
> `webpack =< v3.0.0`目前在`webpack.optimize.UglifyJsPlugin`下包含这个插件的[`v0.4.6`](https://github.com/webpack-contrib/uglifyjs-webpack-plugin/tree/version-0.4) 版本的别名。对于最新版本的使用 \(`v1.0.0`\)，请跟随下面的步骤， `v1.0.0`预计将会在`webpack v4.0.0`中作为`webpack.optimize.UglifyJsPlugin`存在。

## 安装

```js
npm i -D uglifyjs-webpack-plugin
```

## 用法

**webpack.config.js**

```js
const UglifyJSPlugin = require('uglifyjs-webpack-plugin')

module.exports = {
  plugins: [
    new UglifyJSPlugin(options)
  ]
}
```

## 选项

|Name|Type|Default|Description|
|:--:|:--:|:-----:|:----------|
|**`test`**|`{RegExp\|Array<RegExp>}`| <code>/\\.js$/i</code>|Test to match files against|
|**`include`**|`{RegExp\|Array<RegExp>}`|`undefined`|Files to `include`|
|**`exclude`**|`{RegExp\|Array<RegExp>}`|`undefined`|Files to `exclude`|
|**`cache`**|`{Boolean\|String}`|`false`|Enable file caching|
|**`parallel`**|`{Boolean\|Number}`|`false`|Use multi-process parallel running to improve the build speed|
|**`sourceMap`**|`{Boolean}`|`false`|Use source maps to map error message locations to modules (This slows down the compilation) ⚠️ **`cheap-source-map` options don't work with this plugin**|
|**`uglifyOptions`**|`{Object}`|[`{...defaults}`](https://github.com/webpack-contrib/uglifyjs-webpack-plugin/tree/master#uglifyoptions)|`uglify` [Options](https://github.com/mishoo/UglifyJS2/tree/harmony#minify-options)|
|**`extractComments`**|`{Boolean\|RegExp\|Function<(node, comment) -> {Boolean\|Object}>}`|`false`|Whether comments shall be extracted to a separate file, (see [details](https://github.com/webpack/webpack/commit/71933e979e51c533b432658d5e37917f9e71595a) (`webpack >= 2.3.0`)|
|**`warningsFilter`**|`{Function(source) -> {Boolean}}`|`() => true`|Allow to filter uglify warnings|

dsadadasda


|Name|Type|Default|Description|
|:--:|:--:|:-----:|:----------|
|**`ie8`**|`{Boolean}`|`false`|Enable IE8 Support|
|**`ecma`**|`{Number}`|`undefined`|Supported ECMAScript Version (`5`, `6`, `7` or `8`). Affects `parse`, `compress` && `output` options|
|**[`parse`](https://github.com/mishoo/UglifyJS2/tree/harmony#parse-options)**|`{Object}`|`{}`|Additional Parse Options|
|**[`mangle`](https://github.com/mishoo/UglifyJS2/tree/harmony#mangle-options)**|`{Boolean\|Object}`|`true`|Enable Name Mangling (See [Mangle Properties](https://github.com/mishoo/UglifyJS2/tree/harmony#mangle-properties-options) for advanced setups, use with ⚠️)|
|**[`output`](https://github.com/mishoo/UglifyJS2/tree/harmony#output-options)**|`{Object}`|`{}`|Additional Output Options (The defaults are optimized for best compression)|
|**[`compress`](https://github.com/mishoo/UglifyJS2/tree/harmony#compress-options)**|`{Boolean\|Object}`|`true`|Additional Compress Options|
|**`warnings`**|`{Boolean}`|`false`|Display Warnings|


sa

|Name|Type|Default|Description|
|:--:|:--:|:-----:|:----------|
|**`condition`**|`{Regex\|Function}`|``|Regular Expression or function (see previous point)|
|**`filename`**|`{String\|Function}`|`${file}.LICENSE`|The file where the extracted comments will be stored. Can be either a `{String}` or a `{Function<(string) -> {String}>}`, which will be given the original filename. Default is to append the suffix `.LICENSE` to the original filename|
|**`banner`**|`{Boolean\|String\|Function}`|`/*! For license information please see ${filename}.js.LICENSE */`|The banner text that points to the extracted file and will be added on top of the original file. Can be `false` (no banner), a `{String}`, or a `{Function<(string) -> {String}` that will be called with the filename where extracted comments have been stored. Will be wrapped into comment|


