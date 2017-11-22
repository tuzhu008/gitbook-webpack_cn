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

|名称|类型|默认值|描述|
|:--:|:--:|:-----:|:----------|
|**`test`**|`{RegExp\|Array<RegExp>}`| <code>/\\.js$/i</code>|测试匹配的文件|
|**`include`**|`{RegExp\|Array<RegExp>}`|`undefined`| `包含`的文件|
|**`exclude`**|`{RegExp\|Array<RegExp>}`|`undefined`|`排除`的文件|
|**`cache`**|`{Boolean\|String}`|`false`|开启文件缓存来提高构建速度|
|**`parallel`**|`{Boolean\|Number}`|`false`|使用多进程并行运行来提高构建速度|
|**`sourceMap`**|`{Boolean}`|`false`|使用源映射将错误信息位置映射到模块 (这减慢了编译的速度) ⚠️ **`cheap-source-map` 选项不能和这个插件一同工作**|
|**`uglifyOptions`**|`{Object}`|[`{...defaults}`](https://github.com/webpack-contrib/uglifyjs-webpack-plugin/tree/master#uglifyoptions)|`uglify` [选项](https://github.com/mishoo/UglifyJS2/tree/harmony#minify-options)|
|**`extractComments`**|`{Boolean\|RegExp\|Function<(node, comment) -> {Boolean\|Object}>}`|`false`|是否将注释提取到单独的文件， (参见 [细节](https://github.com/webpack/webpack/commit/71933e979e51c533b432658d5e37917f9e71595a) (`webpack >= 2.3.0`)|
|**`warningsFilter`**|`{Function(source) -> {Boolean}}`|`() => true`|允许过滤uglify警告|



```js
new UglifyJSPlugin({
  test: /\.js($|\?)/i,
   include: /\/includes/,
   exclude: /\/excludes/,
   cache: true,// 启用文件缓存。默认缓存目录路径：node_modules/.cache/uglifyjs-webpack-plugin.
})
```
### `cache`
类型：boolean | string
#### `boolean`
当`cache`为布尔值时表示是否启用文件缓存。
默认的缓存目录路径:``node_modules/.cache/uglifyjs-webpack-plugin``。
#### `string`
`cache`还可以指定为表示缓存一个字符串，表示缓存路径。

|名称|类型|默认值|描述|
|:--:|:--:|:-----:|:----------|
|**`ie8`**|`{Boolean}`|`false`|Enable IE8 Support|
|**`ecma`**|`{Number}`|`undefined`|Supported ECMAScript Version (`5`, `6`, `7` or `8`). Affects `parse`, `compress` && `output` options|
|**[`parse`](https://github.com/mishoo/UglifyJS2/tree/harmony#parse-options)**|`{Object}`|`{}`|Additional Parse Options|
|**[`mangle`](https://github.com/mishoo/UglifyJS2/tree/harmony#mangle-options)**|`{Boolean\|Object}`|`true`|Enable Name Mangling (See [Mangle Properties](https://github.com/mishoo/UglifyJS2/tree/harmony#mangle-properties-options) for advanced setups, use with ⚠️)|
|**[`output`](https://github.com/mishoo/UglifyJS2/tree/harmony#output-options)**|`{Object}`|`{}`|Additional Output Options (The defaults are optimized for best compression)|
|**[`compress`](https://github.com/mishoo/UglifyJS2/tree/harmony#compress-options)**|`{Boolean\|Object}`|`true`|Additional Compress Options|
|**`warnings`**|`{Boolean}`|`false`|Display Warnings|


sa

|名称|类型|默认值|描述|
|:--:|:--:|:-----:|:----------|
|**`condition`**|`{Regex\|Function}`|``|Regular Expression or function (see previous point)|
|**`filename`**|`{String\|Function}`|`${file}.LICENSE`|The file where the extracted comments will be stored. Can be either a `{String}` or a `{Function<(string) -> {String}>}`, which will be given the original filename. Default is to append the suffix `.LICENSE` to the original filename|
|**`banner`**|`{Boolean\|String\|Function}`|`/*! For license information please see ${filename}.js.LICENSE */`|The banner text that points to the extracted file and will be added on top of the original file. Can be `false` (no banner), a `{String}`, or a `{Function<(string) -> {String}` that will be called with the filename where extracted comments have been stored. Will be wrapped into comment|


