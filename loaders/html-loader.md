

[![npm][npm]][npm-url]
[![deps][deps]][deps-url]
[![test][test]][test-url]
[![coverage][cover]][cover-url]
[![chat][chat]][chat-url]

<div align="center">
  <img width="200" height="200"
    src="https://worldvectorlogo.com/logos/html5.svg">
  <a href="https://github.com/webpack/webpack">
    <img width="200" height="200" vspace="" hspace="25"
      src="https://worldvectorlogo.com/logos/webpack.svg">
  </a>
</div>
<h1>HTML Loader</h1>

[![](https://img.shields.io/badge/Github-%E6%9F%A5%E7%9C%8B%E6%9B%B4%E5%A4%9A-brightgreen.svg)
](https://github.com/webpack-contrib/html-loader)

导出HTML为字符串。当编译器需要时，HTML 是被压缩的。




<h2>安装</h2>

```bash
npm i -D html-loader
```

<h2>用法</h2>


默认情况下，每个本地的 `<img src="image.png">` 都需要通过 require （`require('./image.png')`）来进行加载。您可能需要在配置中为图片指定 loader（推荐 `file-loader` 或 `url-loader` ）

您可以通过查询参数 `attrs`，来指定哪个标签属性组合(tag-attribute combination)应该被此 loader 处理。传递数组或以空格分隔的 `<tag>`:`<attribute> `组合的列表。（默认值：`attrs=img:src`）

如果使用`<custom-elements>`，而且很多都使用了一个`custom-src`属性，您不必指定每个组合`<tag>:<attribute>`:只需指定一个空的标记，就像`attrs=:custom-src`，它将匹配每个元素。

```js
{
  test: /\.(html)$/,
  use: {
    loader: 'html-loader',
    options: {
      attrs: [':data-src']
    }
  }
}
```

要完全禁用对标签属性的处理（例如，如果你在客户端处理图片加载），你可以传入 `attrs=false`。

<h2 >示例</h2>

使用这个配置:

```js
{
  module: {
    rules: [
      { test: /\.jpg$/, use: [ "file-loader" ] },
      { test: /\.png$/, use: [ "url-loader?mimetype=image/png" ] }
    ]
  },
  output: {
    publicPath: "http://cdn.example.com/[hash]/"
  }
}
```

``` html
<!-- file.html -->
<img src="image.png" data-src="image2x.png" >
```

```js
require("html-loader!./file.html");

// => '<img src="http://cdn.example.com/49eba9f/a992ca.png"
//         data-src="image2x.png">'
```

```js
require("html-loader?attrs=img:data-src!./file.html");

// => '<img src="image.png" data-src="data:image/png;base64,..." >'
```

```js
require("html-loader?attrs=img:src img:data-src!./file.html");
require("html-loader?attrs[]=img:src&attrs[]=img:data-src!./file.html");

// => '<img  src="http://cdn.example.com/49eba9f/a992ca.png"        
//           data-src="data:image/png;base64,..." >'
```

```js
require("html-loader?-attrs!./file.html");

// => '<img  src="image.jpg"  data-src="image2x.png" >'
```

通过运行 `webpack --optimize-minimize` 来最小化

```html
'<img src=http://cdn.example.com/49eba9f/a9f92ca.jpg
      data-src=data:image/png;base64,...>'
```

或者在 `webpack.conf.js` 的 rule 选项中指定 `minimize` 属性

```js
module: {
  rules: [{
    test: /\.html$/,
    use: [ {
      loader: 'html-loader',
      options: {
        minimize: true
      }
    }],
  }]
}
```

在默认情况下最小化的规则如下:
 - removeComments
 - removeCommentsFromCDATA
 - removeCDATASectionsFromCDATA
 - collapseWhitespace
 - conservativeCollapse
 - removeAttributeQuotes
 - useShortDoctype
 - keepClosingSlash
 - minifyJS
 - minifyCSS
 - removeScriptTypeAttributes
 - removeStyleTypeAttributes
 
 你可以在`webpack.conf.js`中使用下面的选项禁用规则：

```js
module: {
  rules: [{
    test: /\.html$/,
    use: [ {
      loader: 'html-loader',
      options: {
        minimize: true,
        removeComments: false,
        collapseWhitespace: false
      }
    }],
  }]
}
```

### 'Root-relative' URLs

对于以 `/` 开头的 url，默认行为是不转换它们。 如果设置了 `root` 查询参数，它将在被添加到 URL 之前，然后进行转换。

和上面配置相同：

``` html
<!-- file.html -->
<img src="/image.jpg">
```

```js
require("html-loader!./file.html");

// => '<img  src="/image.jpg">'
```

```js
require("html-loader?root=.!./file.html");

// => '<img  src="http://cdn.example.com/49eba9f/a992ca.jpg">'
```

### 插值

你可以使用插值（`interpolate`）标记来支持ES6的模板语法。像这样：

```js
require("html-loader?interpolate!./file.html");
```

```html
<img src="${require(`./images/gallery.png`)}">

<div>${require('./components/gallery.html')}</div>
```
如果你只想在模板中使用 `require`，任何其它的 `${}` 不被转换，你可以设置 `interpolate` 标记为 `require`，就像这样：

```js
// 设置 interpolate
require("html-loader?interpolate=require!./file.ftl");
```

```html

<#list list as list>
  <a href="${list.href!}" />${list.name}</a>
</#list>

<img src="${require(`./images/gallery.png`)}">

<div>${require('./components/gallery.html')}</div>
```

### Export 格式

有几个不同的export格式可用：

+ ```module.exports``` (默认, cjs format). "Hello world" 变为 ```module.exports = "Hello world";```
+ ```exports.default``` (当 ```exportAsDefault``` 参数被设置，  es6to5 format). "Hello world" 变为 ```exports.default = "Hello world";```
+ ```export default``` (当 ```exportAsEs6Default``` 参数被设置， es6 格式). "Hello world" 变为 ```export default "Hello world";```

### 高级选项

如果你需要传递[更多高级选项](https://github.com/webpack/html-loader/pull/46), 尤其是那些不能被严格化的，你可以在 `webpack.config.js`上设置一个 `htmlLoader`属性:

```js
var path = require('path')

module.exports = {
  ...
  module: {
    rules: [
      {
        test: /\.html$/,
        use: [ "html-loader" ]
      }
    ]
  },
  htmlLoader: {
    ignoreCustomFragments: [/\{\{.*?}}/],
    root: path.resolve(__dirname, 'assets'),
    attrs: ['img:src', 'link:href']
  }
};
```
如果你需要定义两个不同的loader配置，你可以通过`html-loader?config=otherHtmlLoaderConfig`来改变配置的属性名称：

```js
module.exports = {
  ...
  module: {
    rules: [
      {
        test: /\.html$/,
        use: [ "html-loader?config=otherHtmlLoaderConfig" ]
      }
    ]
  },
  otherHtmlLoaderConfig: {
    ...
  }
};
```

### 导出到Html文件

A very common scenario is exporting the HTML into their own _.html_ file, to
serve them directly instead of injecting with javascript. This can be achieved
with a combination of 3 loaders:

- [file-loader](https://github.com/webpack/file-loader)
- [extract-loader](https://github.com/peerigon/extract-loader)
- html-loader

The html-loader will parse the URLs, require the images and everything you
expect. The extract loader will parse the javascript back into a proper html
file, ensuring images are required and point to proper path, and the file loader
will write the _.html_ file for you. Example:

```js
{
  test: /\.html$/,
  use: [ 'file-loader?name=[path][name].[ext]!extract-loader!html-loader' ]
}
```

<h2 align="center">Maintainers</h2>

<table>
  <tbody>
    <tr>
      <td align="center">
        <img width="150" height="150"
        src="https://avatars.githubusercontent.com/u/18315?v=3">
        </br>
        <a href="https://github.com/hemanth">Hemanth</a>
      </td>
      <td align="center">
        <img width="150" height="150"
        src="https://avatars.githubusercontent.com/u/8420490?v=3">
        </br>
        <a href="https://github.com/d3viant0ne">Joshua Wiens</a>
      </td>
      <td align="center">
        <img width="150" height="150" src="https://avatars.githubusercontent.com/u/5419992?v=3">
        </br>
        <a href="https://github.com/michael-ciniawsky">Michael Ciniawsky</a>
      </td>
      <td align="center">
        <img width="150" height="150"
        src="https://avatars.githubusercontent.com/u/6542274?v=3">
        </br>
        <a href="https://github.com/imvetri">Imvetri</a>
      </td>
    </tr>
    <tr>
      <td align="center">
        <img width="150" height="150"
        src="https://avatars.githubusercontent.com/u/1520965?v=3">
        </br>
        <a href="https://github.com/andreicek">Andrei Crnković</a>
      </td>
      <td align="center">
        <img width="150" height="150"
        src="https://avatars.githubusercontent.com/u/3367801?v=3">
        </br>
        <a href="https://github.com/abouthiroppy">Yuta Hiroto</a>
      </td>
      <td align="center">
        <img width="150" height="150" src="https://avatars.githubusercontent.com/u/80044?v=3">
        </br>
        <a href="https://github.com/petrunov">Vesselin Petrunov</a>
      </td>
      <td align="center">
        <img width="150" height="150"
        src="https://avatars.githubusercontent.com/u/973543?v=3">
        </br>
        <a href="https://github.com/gajus">Gajus Kuizinas</a>
      </td>
    </tr>
  </tbody>
</table>


[npm]: https://img.shields.io/npm/v/html-loader.svg
[npm-url]: https://npmjs.com/package/html-loader

[deps]: https://david-dm.org/webpack/html-loader.svg
[deps-url]: https://david-dm.org/webpack/html-loader

[chat]: https://img.shields.io/badge/gitter-webpack%2Fwebpack-brightgreen.svg
[chat-url]: https://gitter.im/webpack/webpack

[test]: http://img.shields.io/travis/webpack/html-loader.svg
[test-url]: https://travis-ci.org/webpack/html-loader

[cover]: https://codecov.io/gh/webpack/html-loader/branch/master/graph/badge.svg
[cover-url]: https://codecov.io/gh/webpack/html-loader
