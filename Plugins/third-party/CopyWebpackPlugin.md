<div align="center">
  <a href="https://github.com/webpack/webpack">
    <img width="200" height="200"
      src="https://webpack.js.org/assets/icon-square-big.svg">
  </a>
</div>
<h1>Copy Webpack Plugin</h1>
将单个文件或整个目录复制到构建目录

[![](https://img.shields.io/badge/Github-%E6%9F%A5%E7%9C%8B%E6%9B%B4%E5%A4%9A-brightgreen.svg)](https://github.com/webpack-contrib/copy-webpack-plugin)

<h2>安装</h2>

```
npm install --save-dev copy-webpack-plugin
```

<h2>用法</h2>
```js
new CopyWebpackPlugin(patterns, options)
```
### patterns
类型：array

模式数组

一个模式的样子:
`{ from: 'source', to: 'dest' }`

或者，在简单的情况下，仅指定一个`from`，使用默认目的地。您可以使用简单字符串而不是对象:
`'source'`

#### 模式属性:

| 名称 | 是否必须 | 默认值     | 细节                                             |
|------|----------|------------ |---------------------------------------------------------|
| `from` | Y        |             | _例如:_<br>'relative/file.txt'<br>'/absolute/file.txt'<br>'relative/dir'<br>'/absolute/dir'<br>'\*\*/\*'<br>{glob:'\*\*/\*', dot: true}<br><br>Globs 接收 [minimatch options](https://github.com/isaacs/minimatch) |
| `to`   | N        | 如果 `from` 是一个文件或者目录，输出root<br><br> 如果 `from` 是 glob，已解决的glob路径| _例如:_<br>'relative/file.txt'<br>'/absolute/file.txt'<br>'relative/dir'<br>'/absolute/dir'<br>'relative/[name].[ext]'<br>'/absolute/[name].[ext]'<br><br>模板是 [file-loader patterns](https://github.com/webpack/file-loader) |
| `toType` | N | 为**'文件'** ，如果`to` 有扩展或者`from`是文件 <br><br>为**'目录'**， 如果 `from` 是目录, `to` 不包含扩展或者末尾是 '/'<br><br>**为'template'** ，如果 `to`包含  [一个模板模式](https://github.com/webpack/file-loader) | |
| `context` | N | options.context \|\| compiler.options.context | 一个决定如何解释`from`路径的路径 |
| `flatten` | N | false | 删除所有的目录引用，并且只复制文件名。<br><br>如果文件具有相同的名称，则结果是不确定的 |
| `ignore` | N | [] |  对于这种模式，忽略的额外globs |
| `transform` | N | function(content, path) {<br>&nbsp;&nbsp;return content;<br>} | 在写入webpack之前修改文件内容的函数|
| `force` | N | false | 重写已经在compilation.assets中的文件(通常由其他插件添加)|

#### 可用的选项:

| 名称 | 默认值 | 细节 |
| ---- | ------- | ------- |
| `context` | compiler.options.context | 一个决定如何说明`from`路径的路径，为所有模式所共享|
| `ignore` | [] | 忽略的glob数组 (应用于 `from`) |
| `copyUnmodified` | false | 复制文件，不考虑什么时候使用watch或webpack-devserver。所有文件都是在第一个构建中复制的，不管这个选项是什么。|
| `debug` | **'warning'** | _选项:_<br>**'warning'** - 仅警告<br>**'info'** 或者 true - 文件地址和读取信息<br>**'debug'** - 非常详细的调试信息

### 示例

```javascript
var CopyWebpackPlugin = require('copy-webpack-plugin');
var path = require('path');

module.exports = {
    context: path.join(__dirname, 'app'),
    devServer: {
        // 这是webpack-dev-server的旧版本所需要的
        // 如果你使用绝对的'to' 路径。路径应该是通往您的构建目的地的绝对路径。
        outputPath: path.join(__dirname, 'build')
    },
    plugins: [
        new CopyWebpackPlugin([
            // {output}/file.txt
            { from: 'from/file.txt' },
            
            // 等效
            'from/file.txt',

            // {output}/to/file.txt
            { from: 'from/file.txt', to: 'to/file.txt' },
            
            // {output}/to/directory/file.txt
            { from: 'from/file.txt', to: 'to/directory' },

            // 拷贝目录内容到 {output}/
            { from: 'from/directory' },
            
            // 拷贝目录内容到 {output}/to/directory/
            { from: 'from/directory', to: 'to/directory' },
            
            // 拷贝 glob 结果到 /absolute/path/
            { from: 'from/directory/**/*', to: '/absolute/path' },

            // 拷贝 glob (和 dot 文件) 到 /absolute/path/
            {
                from: {
                    glob:'from/directory/**/*',
                    dot: true
                },
                to: '/absolute/path'
            },

            // 拷贝 glob 结果, 相对于 context
            {
                context: 'from/directory',
                from: '**/*',
                to: '/absolute/path'
            },
            
            // {output}/file/without/extension
            {
                from: 'path/to/file.txt',
                to: 'file/without/extension',
                toType: 'file'
            },
            
            // {output}/directory/with/extension.ext/file.txt
            {
                from: 'path/to/file.txt',
                to: 'directory/with/extension.ext',
                toType: 'dir'
            }
        ], {
            ignore: [
                // 不拷贝任何带txt扩展的文件
                '*.txt',
                
                // 不要复制任何文件，即使它们从一个点开始
                '**/*',

                // 不复制任何文件，除非他们以一个点开始
                { glob: '**/*', dot: false }
            ],

            // 默认情况下，我们只在观察或webpack-dev-server构建期间复制修改过的文件。将此设置为`true`复制所有文件。
            copyUnmodified: true
        })
    ]
};
```

### 常见问题

#### "EMFILE: too many open files" or "ENFILE: file table overflow"

使用 [graceful-fs](https://www.npmjs.com/package/graceful-fs)全局补丁

`npm install graceful-fs --save-dev`

在您的webpack配置文件的顶部，插入这个

    var fs = require('fs');
    var gracefulFs = require('graceful-fs');
    gracefulFs.gracefulify(fs);

参见 [this issue](https://github.com/kevlened/copy-webpack-plugin/issues/59#issuecomment-228563990) 获取更多细节

#### This doesn't copy my files with webpack-dev-server（使用webpack-devserver时不会复制我的文件）

从版本 [3.0.0](https://github.com/kevlened/copy-webpack-plugin/blob/master/CHANGELOG.md#300-may-14-2016)开始, 我们停止使用fs来拷贝文件到文件系统，并且开始依赖于webpack的[in-memory filesystem](https://webpack.github.io/docs/webpack-dev-server.html#content-base):

> ... webpack-dev-server 服务于你的构建文件夹的静态文件。它将监视您的源文件的更改，将重新编译包。**这个修改后的包是在公共路径中指定的相对路径(参见API)中提供的**。它不会被写入您配置的输出目录.

如果必须将webpack-dev-server写入输出目录，你可以使用[write-file-webpack-plugin](https://github.com/gajus/write-file-webpack-plugin)强制它这样做。

<h2 align="center">Maintainers</h2>

<table>
  <tbody>
    <tr>
      <td align="center">
        <a href="https://github.com/bebraw">
          <img width="150" height="150" src="https://github.com/bebraw.png?v=3&s=150">
          </br>
          Juho Vepsäläinen
        </a>
      </td>
      <td align="center">
        <a href="https://github.com/d3viant0ne">
          <img width="150" height="150" src="https://github.com/d3viant0ne.png?v=3&s=150">
          </br>
          Joshua Wiens
        </a>
      </td>
      <td align="center">
        <a href="https://github.com/michael-ciniawsky">
          <img width="150" height="150" src="https://github.com/michael-ciniawsky.png?v=3&s=150">
          </br>
          Michael Ciniawsky
        </a>
      </td>
      <td align="center">
        <a href="https://github.com/evilebottnawi">
          <img width="150" height="150" src="https://github.com/evilebottnawi.png?v=3&s=150">
          </br>
          Alexander Krasnoyarov
        </a>
      </td>
    </tr>
  <tbody>
</table>


[npm]: https://img.shields.io/npm/v/copy-webpack-plugin.svg
[npm-url]: https://npmjs.com/package/copy-webpack-plugin

[node]: https://img.shields.io/node/v/copy-webpack-plugin.svg
[node-url]: https://nodejs.org

[deps]: https://david-dm.org/webpack-contrib/copy-webpack-plugin.svg
[deps-url]: https://david-dm.org/webpack-contrib/copy-webpack-plugin

[test]: https://secure.travis-ci.org/webpack-contrib/copy-webpack-plugin.svg
[test-url]: http://travis-ci.org/webpack-contrib/copy-webpack-plugin

[cover]: https://codecov.io/gh/webpack-contrib/copy-webpack-plugin/branch/master/graph/badge.svg
[cover-url]: https://codecov.io/gh/webpack-contrib/copy-webpack-plugin

[chat]: https://img.shields.io/badge/gitter-webpack%2Fwebpack-brightgreen.svg
[chat-url]: https://gitter.im/webpack/webpack