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

#### `filename`

Where the manifest will be exported to on bundle compilation. This will be relative to the main webpack output directory. Default = `"manifest.json"`

#### `manifestVariable`

What JS variable on the client webpack should refer to when requiring chunks. Default = `"webpackManifest"`

#### `inlineManifest`

Whether or not to write the manifest output into the [html-webpack-plugin](https://github.com/ampedandwired/html-webpack-plugin). Default = `false`

```html
// index.ejs
<body>
    <!-- app -->
    <%= htmlWebpackPlugin.files.webpackManifest %>
</body>
```



