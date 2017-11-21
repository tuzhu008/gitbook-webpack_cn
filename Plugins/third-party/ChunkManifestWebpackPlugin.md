# chunk-manifest-webpack-plugin



允许导出一个JSON文件，该文件将chunk id映射到其生成的资源文件。Webpack可以读取这个映射，假设它是在客户机上提供的，而不是在引导脚本中存储一个映射\(带有哈希化的chunk资产\)，这样就可以真正地利用长期缓存。

## 用法

通过npm安装：

```shell
npm install --save-dev chunk-manifest-webpack-plugin
```

通过yarn安装：

```shell
yarn add --dev chunk-manifest-webpack-plugin
```

然后require它，并提供到webpack:

```js
//  webpack.config.js 或类似的文件
const ChunkManifestPlugin = require('chunk-manifest-webpack-plugin');

module.exports = {
  // 你的配置
  plugins: [
    new ChunkManifestPlugin({
      filename: 'manifest.json',
      manifestVariable: 'webpackManifest',
      inlineManifest: false
    })
  ]
};
```

### Options

类型：Object

#### `filename`

清单将被导出到包编译的地方。这将相对于主webpack输出目录。默认=` " manifest.json "`

#### `manifestVariable`

当请求chunk时，客户端webpack上的JS变量应该是什么。默认= `" webpackManifest "`

#### `inlineManifest`

是否将清单输出写入[html-webpack-plugin](https://github.com/ampedandwired/html-webpack-plugin)。默认= `false`

```html
// index.ejs
<body>
    <!-- app -->
    <%= htmlWebpackPlugin.files.webpackManifest %>
</body>
```



