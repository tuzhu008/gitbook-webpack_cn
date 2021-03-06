# ExtractTextWebpackPlugin

[![](https://img.shields.io/badge/Github-查看更多-brightgreen.svg)](https://github.com/webpack-contrib/extract-text-webpack-plugin)

将文本从一个bundle或多个bundle中提取到一个单独的文件中。

## 安装

```bash
# for webpack 3
npm install --save-dev extract-text-webpack-plugin
# for webpack 2
npm install --save-dev extract-text-webpack-plugin@2.1.2
# for webpack 1
npm install --save-dev extract-text-webpack-plugin@1.0.1
```

## 用法

> **\[warning\]** 警告：
>
> 对于 webpack v1, 请查看[分支为 webpack-1 的 README 文档](https://github.com/webpack/extract-text-webpack-plugin/blob/webpack-1/README.md)。

```js
const ExtractTextPlugin = require("extract-text-webpack-plugin");

module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ExtractTextPlugin.extract({
          fallback: "style-loader",
          use: "css-loader"
        })
      }
    ]
  },
  plugins: [
    new ExtractTextPlugin("styles.css"),
  ]
}
```

它会将所有的入口 chunk\(entry chunks\)中引用的`*.css`，移动到独立分离的 CSS 文件。因此，你的样式将不再内嵌到 JS bundle 中，而是会放到一个单独的 CSS 文件（即`styles.css`）当中。 如果你的样式文件大小较大，这会做更快提前加载，因为 CSS bundle 会跟 JS bundle 并行加载。

| 优点 | 缺点 |
| :--- | :--- |
| 更少的style标签（旧版本ie对style标签数量有限制） | 额外的Http请求 |
| CSS SourceMap\(使用 devtool: "source-map" 和 extract-text-webpack-plugin?sourceMap 配置\) | 更长的编译事件 |
| CSS请求并行 | 没有运行时（runtime）的公共路径修改 |
| CSS单独缓存 | 没有热替换 |
| 更快的浏览器运行时\(runtime\) \(更少代码和 DOM 操作\) | ... |

## 选项

```js
new ExtractTextPlugin(options: filename | object)
```

类型：string \| object



### `string`:

当传入一个字符串时，这个字符串代表的是输出的文件名（filename）

### `object`:

| 名称 | 类型 | 描述 |
| :--- | :--- | :--- |
| `id` | string | 此插件实例的唯一 ident。（仅限高级用途，默认情况下自动生成） |
| `filename` | string \| function | 生成文件的文件名。可能包含 `[name]`, `[id]` 和 `[contenthash]` |
| `allChunks` | boolean | 从所有额外的 chunk\(additional chunk\) 提取（默认情况下，它仅从初始chunk\(initial chunk\) 中提取）当使用 CommonsChunkPlugin 并且在公共 chunk 中有提取的 chunk（来自ExtractTextPlugin.extract）时，`allChunks`** 必须 **设置为 true |
| `disable` | boolean | 禁用插件 |
| `ignoreOrder` | boolean | 禁用顺序检查（这对CSS模块很有用），默认false |

`[name]`chunk 的名称

* `[id]`chunk 的数量
* `[contenthash]`根据提取文件的内容生成的 hash
* `[<hashType>:contenthash:<digestType>:length>]`你也可以选择配置
  * 其他`hashType`： 比如`sha1`,`md5`,`sha256`,`sha512`
  * 其他`digestType`：比如.`hex`,`base26`,`base32`,`base36`,`base49`,`base52`,`base58`,`base62`,`base64`
  * `length`：中哈希的长度

> **\[warning\]** 注：  
>  `ExtractTextPlugin`对**每个入口 chunk**都生成一个对应的文件，所以当你配置多个入口 chunk 的时候，你必须使用`[name]`,`[id]`或`[contenthash]`

#### `extract`

```js
ExtractTextPlugin.extract(options: loader | object)
```

从一个已存在的 loader 中，创建一个提取\(extract\) loader。支持的 loader 类型`{ loader: [name]-loader ->{String}, options: {}> {Object} }`。

| Name | Type | Description |
| :---: | :---: | :--- |
| `options.use` | `{String}`/`{Array}`/`{Object}` | loader 被用于将资源转换成一个 CSS 导出模块 \(**必填**\) |
| `options.fallback` | `{String}`/`{Array}`/`{Object}` | loader（例如 'style-loader'）应用于当 CSS 没有被提取\(也就是一个额外的 chunk，当 allChunks: false\) |
| `options.publicPath` | `{String}` | 重写此 loader 的 publicPath 配置 |



### 多个实例（instance）

每个`ExtractTextPlugin`实例上都有一个`extract`函数。如果有多于一个`ExtractTextPlugin`实例的情形，你应该使用这个函数。

```js
const ExtractTextPlugin = require('extract-text-webpack-plugin');

// 创建多个实例
const extractCSS = new ExtractTextPlugin('stylesheets/[name]-one.css');
const extractLESS = new ExtractTextPlugin('stylesheets/[name]-two.css');

module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: extractCSS.extract([ 'css-loader', 'postcss-loader' ])
      },
      {
        test: /\.less$/i,
        use: extractLESS.extract([ 'css-loader', 'less-loader' ])
      },
    ]
  },
  plugins: [
    extractCSS,
    extractLESS
  ]
};
```

### 提取 Sass 或 LESS

配置和上面是相同的，需要时可以将`sass-loader`切换为`less-loader`。

```js
const ExtractTextPlugin = require('extract-text-webpack-plugin');

module.exports = {
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: ExtractTextPlugin.extract({
          fallback: 'style-loader',
          //如果需要，可以在 sass-loader 之前将 resolve-url-loader 链接进来
          use: ['css-loader', 'sass-loader']
        })
      }
    ]
  },
  plugins: [
    new ExtractTextPlugin('style.css')
    //如果想要传入选项，你可以这样做：
    //new ExtractTextPlugin({
    //  filename: 'style.css'
    //})
  ]
}

```

### 修改文件名

`filename`参数可以是`Function`。它通过`getPath`来处理格式，如`css/[name].css`，并返回真实的文件名，你可以用`css`替换`css/js`，你会得到新的路径`css/a.css`。

```js
entry: {
  'js/a': "./a"
},
plugins: [
  new ExtractTextPlugin({
    filename:  (getPath) => {
      return getPath('css/[name].css').replace('css/js', 'css');
    },
    allChunks: true
  })
]
```



