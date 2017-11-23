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
  sourceMap: true,
  warningsFilter: (src) => true
})
```
### `cache`
类型：boolean | string
#### `boolean`
当`cache`为布尔值时表示是否启用文件缓存。
默认的缓存目录路径:``node_modules/.cache/uglifyjs-webpack-plugin``。
```js
new UglifyJSPlugin({
  cache: true // 启用缓存
})
```

#### `string`
`cache`还可以指定为一个字符串，表示缓存路径。
```js
new UglifyJSPlugin({
  cache: 'path/to/cache' // 启用缓存，并指定缓存路径
})
```

### `parallel`
类型：boolean | number
#### `Boolean`
实现并行化。默认并发运行的数量: `os.cpus().length - 1`.


```js
 new UglifyJSPlugin({
    parallel: true
  })

```

#### `number`
也可直接指定并发运行数量。

```
new UglifyJSPlugin({
    parallel: 4
  })
```

> **[info]** 注：
>
>并行化可以显著提高您的构建速度，因此高度推荐

### `uglifyOptions`

|名称|类型|默认值|描述|
|:--:|:--:|:-----:|:----------|
|**`ie8`**|`{Boolean}`|`false`|启用 IE8 支持|
|**`ecma`**|`{Number}`|`undefined`|支持 ECMAScript 版本 (`5`, `6`, `7` or `8`). 影响 `parse`, `compress` && `output` 选项|
|**[`parse`](https://github.com/mishoo/UglifyJS2/tree/harmony#parse-options)**|`{Object}`|`{}`|额外的解析选项|
|**[`mangle`](https://github.com/mishoo/UglifyJS2/tree/harmony#mangle-options)**|`{Boolean\|Object}`|`true`|启用名称改编Enable Name Mangling (参见 [Mangle Properties](https://github.com/mishoo/UglifyJS2/tree/harmony#mangle-properties-options) 获取高级设置，请谨慎使用 ⚠️)|
|**[`output`](https://github.com/mishoo/UglifyJS2/tree/harmony#output-options)**|`{Object}`|`{}`|额外的输出选项 (默认值是为了最佳压缩而优化的)|
|**[`compress`](https://github.com/mishoo/UglifyJS2/tree/harmony#compress-options)**|`{Boolean\|Object}`|`true`|额外的压缩选项|
|**`warnings`**|`{Boolean}`|`false`|显示警告|



```js
  new UglifyJSPlugin({
    uglifyOptions: {
      ie8: false,
      ecma: 8,
      parse: {...options},
      mangle: {
        ...options,
        properties: {
          // mangle property options
        }
      },
      output: {
        comments: false,
        beautify: false,
        ...options
      },
      compress: {...options},
      warnings: false
    }
  })

```

### `extractComments`
类型：`Boolean` | `RegExp|String` or `Function<(node, comment) -> {Boolean}>` | `object`

#### `boolean`

`comments`选项将保存的所有注释将被移动到一个单独的文件中。如果原始文件被命名为foo.js，然后这些注释将被存储到`foo.js.LICENSE`。

#### `RegExp|String` or `Function<(node, comment) -> {Boolean}>`
所有与给定表达式匹配的注释(分别被函数计算为`true`)将被提取到单独的文件中。`comments`选项指定是否保留注释，也就是说，在提取其他注释时保存一些注释(例如释文)，或者保存
已经提取的注释。 

|名称|类型|默认值|描述|
|:--:|:--:|:-----:|:----------|
|**`condition`**|`{Regex\|Function}`|``|正则表达式或函数 (参见之前的要点)|
|**`filename`**|`{String\|Function}`|`${file}.LICENSE`|被提取的注释文件将要被存储到的地方。可以是一个`{String}` 或者一个`{Function<(string) -> {String}>}`，得到的将是一个原始文件名。默认是将`.LICENSE`后缀添加到原始文件名|
|**`banner`**|`{Boolean\|String\|Function}`|`/*! For license information please see ${filename}.js.LICENSE */`| banner 文本是指向提取出的文件并将会被添加到原始文件的顶部。可以是 `false`（表示没有 banner），一个 `{String}`，或者一个 `{Function<(string) -> {String}` 返回字符串的函数，会在提取出的文件被存储时候调用。将被包装成注释。|

#### `object`

|名称|类型|默认值|描述|
|:--:|:--:|:-----:|:----------|
| `condition` |`{Regex or Function}`| `` | 通常表达式或者相应函数（见上文）|
| `filename` | `{String or Function}` | `compilation.assets[file]` | 提取注释的文件会被存储。可以使一个 `{String} `字符串或者是一个 `{Function<(string) -> {String}>}` 返回字符串的函数，作为原文件名。默认加上文件后缀名 `.LICENSE` |
| banner | {Boolean|String|Function} | `/*! For license information please see ${filename}.js.LICENSE */` | banner 文本会在原文件的头部指出被提取的文件，会在源文件加入该信息。可以是 `false`（表示没有 banner），一个 `{String}`，或者一个 `{Function<(string) -> {String}` 返回字符串的函数，会在提取已经被存储注释的时候被调用。注释会被覆盖。 |
