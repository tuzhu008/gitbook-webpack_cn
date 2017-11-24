[![npm][npm]][npm-url]
[![node][node]][node-url]
[![deps][deps]][deps-url]
[![chat][chat]][chat-url]

<div align="center">
  <a href="https://github.com/webpack/webpack">
    <img width="200" height="200"
      src="https://webpack.js.org/assets/icon-square-big.svg">
  </a>
</div>

<h1>Style Loader</h1>

[![](https://img.shields.io/badge/Github-%E6%9F%A5%E7%9C%8B%E6%9B%B4%E5%A4%9A-brightgreen.svg)
](https://github.com/webpack-contrib/style-loader)

通过注入一个`<style>`，将CSS添加到DOM



<h2>安装</h2>

```bash
npm install style-loader --save-dev
```

<h2>用法</h2>

建议将 style-loader 与 [css-loader](//loaders/css-loader.md) 结合使用

**component.js**
```js
import style from './file.css'
```

**webpack.config.js**
```js
{
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          { loader: "style-loader" },
          { loader: "css-loader" }
        ]
      }
    ]
  }
}
```

#### `局部 (CSS Modules)`

When using 当使用[局部作用域 CSS](https://github.com/webpack/css-loader#css-scope) 时，模块导出生成的（局部）标识符(identifier)。

**component.js**
```js
import style from './file.css'

style.className === "z849f98ca812"
```

### `Url`

也可以添加一个URL `<link href="path/to/file.css" rel="stylesheet">`，而不是用带有 `<style></style>` 标签的内联 CSS `{String}`。

```js
import url from 'file.css'
```

**webpack.config.js**
```js
{
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          { loader: "style-loader/url" },
          { loader: "file-loader" }
        ]
      }
    ]
  }
}
```

```html
<link rel="stylesheet" href="path/to/file.css">
```

> **[info]** 信息来源:
> 
> Source map 和使用 `url` 引用的资源：当 style-loader 与 `{ options: { sourceMap: true } }` 选项一起使用时，CSS 模块将生成为 `Blob`，因此相对路径无法正常工作（他们将相对于 `chrome:blob` 或 `chrome:devtools`）。为了使资源保持正确的路径，必须设置 webpack 配置中的 `output.publicPath` 属性，以便生成绝对路径。或者，您可以启用上面提到的 `convertToAbsoluteUrls` 选项。

### `可用的（useable）`

按照惯例，引用计数器 API(`Reference Counter API`) 应关联到 `.useable.css`，而 `.css` 应该使用基本的 `style-loader` 用法进行加载。（类似于其他文件类型，例如 `.useable.less` 和 `.less`）。

**webpack.config.js**
```js
{
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          { loader: "style-loader" },
          { loader: "css-loader" },
        ],
      },
      {
        test: /\.useable\.css$/,
        use: [
          {
            loader: "style-loader/useable"
          },
          { loader: "css-loader" },
        ],
      },
    ],
  },
}
```

#### `引用计数器 API`

**component.js**
```js
import style from './file.css'

style.use(); // = style.ref();
style.unuse(); // = style.unref();
```

样式不会添加在 `import/require()` 上，而是在调用 `use/ref` 时添加。如果 `unuse/unref` 与 `use/ref` 一样频繁地被调用，那么样式将从页面中删除。

> **[warning]** 警告: 
>
>当 `unuse/unref` 比 `use/ref` 调用频繁的时候，产生的行为是不确定的。所以不要这样做。

<h2 >选项</h2>

|名称|类型|默认值|描述|
|:--:|:--:|:-----:|:----------|
|**`hmr`**|`{Boolean}`|`true`| 启用/禁用 模块热替换 (HMR), 如果禁用，不会添加 HMR代码(对非本地开发/生产有好处)|
|**`base`** |`{Number}`|`true`| 设置模块 ID 基础(DLLPlugin)|
|**`attrs`**|`{Object}`|`{}`| 添加自定义属性（attrs）到`<style></style>`|
|**`transform`** |`{Function}`|`false`|通过传递一个 转换/条件 来 转换/有条件地 加载CSS |
|**`insertAt`**|`{String\|Object}`|`bottom`|Inserts `<style></style>` at the given position|
|**`insertInto`**|`{String}`|`<head>`|在给定位置插入 `<style></style>` |
|**`sourceMap`**|`{Boolean}`|`false`|启用/禁用 Sourcemaps|
|**`convertToAbsoluteUrls`**|`{Boolean}`|`false`| 当 source maps 被启用时，将相对路径转换为绝对路径|

### `hmr`

Enable/disable Hot Module Replacement (HMR), if disabled no HMR Code will be added.
This could be used for non local development and production.
启用/禁用 模块热替换，如果

**webpack.config.js**
```js
{
  loader: 'style-loader',
  options: {
    hmr: false
  }
}
```

### `base`

This setting is primarily used as a workaround for [css clashes](https://github.com/webpack-contrib/style-loader/issues/163) when using one or more [DllPlugin](https://robertknight.github.io/posts/webpack-dll-plugins/)'s.  `base` allows you to prevent either the *app*'s css (or *DllPlugin2*'s css) from overwriting *DllPlugin1*'s css by specifying a css module id base which is greater than the range used by *DllPlugin1* e.g.:

**webpack.dll1.config.js**
```js
{
  test: /\.css$/,
  use: [
    'style-loader',
    'css-loader'
  ]
}
```

**webpack.dll2.config.js**
```js
{
  test: /\.css$/,
  use: [
    { loader: 'style-loader', options: { base: 1000 } },
    'css-loader'
  ]
}
```

**webpack.app.config.js**
```
{
  test: /\.css$/,
  use: [
    { loader: 'style-loader', options: { base: 2000 } },
    'css-loader'
  ]
}
```

### `attrs`

If defined, style-loader will attach given attributes with their values on `<style>` / `<link>` element.

**component.js**
```js
import style from './file.css'
```

**webpack.config.js**
```js
{
  test: /\.css$/,
  use: [
    { loader: 'style-loader', options: { attrs: { id: 'id' } } }
    { loader: 'css-loader' }
  ]
}
```

```html
<style id="id"></style>
```

#### `Url`

**component.js**
```js
import link from './file.css'
```

**webpack.config.js**
```js
{
  test: /\.css$/,
  use: [
    { loader: 'style-loader/url', options: { attrs: { id: 'id' } } }
    { loader: 'file-loader' }
  ]
}
```

### `transform`

A `transform` is a function that can modify the css just before it is loaded into the page by the style-loader.
This function will be called on the css that is about to be loaded and the return value of the function will be loaded into the page instead of the original css.
If the return value of the `transform` function is falsy, the css will not be loaded into the page at all.

**webpack.config.js**
```js
{
  loader: 'style-loader',
  options: {
    transform: 'path/to/transform.js'
  }
}
```

**transform.js**
```js
module.exports = function (css) {
  // Here we can change the original css
  const transformed = css.replace('.classNameA', '.classNameB')

  return transformed
}
```

#### `Conditional`

**webpack.config.js**
```js
{
  loader: 'style-loader',
  options: {
    transform: 'path/to/conditional.js'
  }
}
```

**conditional.js**
```js
module.exports = function (css) {
  // If the condition is matched load [and transform] the CSS
  if (css.includes('something I want to check')) {
    return css;
  }
  // If a falsy value is returned, the CSS won't be loaded
  return false
}
```

### `insertAt`

By default, the style-loader appends `<style>` elements to the end of the style target, which is the `<head>` tag of the page unless specified by `insertInto`. This will cause CSS created by the loader to take priority over CSS already present in the target. To insert style elements at the beginning of the target, set this query parameter to 'top', e.g

**webpack.config.js**
```js
{
  loader: 'style-loader',
  options: {
    insertAt: 'top'
  }
}
```

A new `<style>` element can be inserted before a specific element by passing an object, e.g.

**webpack.config.js**
```js
{
  loader: 'style-loader',
  options: {
    insertAt: {
        before: '#id'
    }
  }
}
```

### `insertInto`
By default, the style-loader inserts the `<style>` elements into the `<head>` tag of the page. If you want the tags to be inserted somewhere else you can specify a CSS selector for that element here. If you target an [IFrame](https://developer.mozilla.org/en-US/docs/Web/API/HTMLIFrameElement) make sure you have sufficient access rights, the styles will be injected into the content document head.
You can also insert the styles into a [ShadowRoot](https://developer.mozilla.org/en-US/docs/Web/API/ShadowRoot), e.g

**webpack.config.js**
```js
{
  loader: 'style-loader',
  options: {
    insertInto: '#host::shadow>#root'
  }
}
```

### `singleton`

If defined, the style-loader will reuse a single `<style>` element, instead of adding/removing individual elements for each required module.

> ℹ️  This option is on by default in IE9, which has strict limitations on the number of style tags allowed on a page. You can enable or disable it with the singleton option.

**webpack.config.js**
```js
{
  loader: 'style-loader',
  options: {
    singleton: true
  }
}
```

### `sourceMap`

Enable/Disable source map loading

**webpack.config.js**
```js
{
  loader: 'style-loader',
  options: {
    sourceMap: true
  }
}
```

### `convertToAbsoluteUrls`

If convertToAbsoluteUrls and sourceMaps are both enabled, relative urls will be converted to absolute urls right before the css is injected into the page. This resolves [an issue](https://github.com/webpack/style-loader/pull/96) where relative resources fail to load when source maps are enabled. You can enable it with the convertToAbsoluteUrls option.

**webpack.config.js**
```js
{
  loader: 'style-loader',
  options: {
    sourceMap: true,
    convertToAbsoluteUrls: true
  }
}
```

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
        <a href="https://github.com/sapegin">
          <img width="150" height="150" src="https://github.com/sapegin.png?v=3&s=150">
          </br>
          Artem Sapegin
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
    <tr>
      <td align="center">
        <a href="https://github.com/sokra">
          <img width="150" height="150" src="https://github.com/sokra.png?v=3&s=150">
          </br>
          Tobias Koppers
        </a>
      </td>
      <td align="center">
        <a href="https://github.com/SpaceK33z">
          <img width="150" height="150" src="https://github.com/SpaceK33z.png?v=3&s=150">
          </br>
          Kees Kluskens
        </a>
      </td>
    <tr>
  <tbody>
</table>


[npm]: https://img.shields.io/npm/v/style-loader.svg
[npm-url]: https://npmjs.com/package/style-loader

[node]: https://img.shields.io/node/v/style-loader.svg
[node-url]: https://nodejs.org

[deps]: https://david-dm.org/webpack/style-loader.svg
[deps-url]: https://david-dm.org/webpack/file-loader

[chat]: https://badges.gitter.im/webpack/webpack.svg
[chat-url]: https://gitter.im/webpack/webpack
