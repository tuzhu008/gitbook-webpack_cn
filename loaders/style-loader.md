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

启用/禁用 模块热替换，如果禁用，HRM不会被添加。
这可以用于非本地开发和生产。

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

当使用一个或多个 [DllPlugin](https://robertknight.github.io/posts/webpack-dll-plugins/) 时，此设置主要用作 [css 冲突](https://github.com/webpack-contrib/style-loader/issues/163) 的修补方案。`base` 可以防止 *app* 的 css（或 *DllPlugin2* 的 css）覆盖 *DllPlugin1* 的 css，方法是指定一个大于*DllPlugin1*使用的范围的 css 模块id ，例如：

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
```js
{
  test: /\.css$/,
  use: [
    { loader: 'style-loader', options: { base: 2000 } },
    'css-loader'
  ]
}
```

### `attrs`

如果被定义，style-loader在`<style>` / `<link>`元素上附加给定的属性以及他们的值。

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

`transform` 是一个函数，可以在通过 style-loader 来在css被加载到页面之前修改它。 该函数将在即将加载的 css 上调用，函数的返回值将被加载到页面，而不是原始的 css 。 如果 `transform` 函数的返回值是 falsy 值，那么 css 根本就不会加载到页面中。

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
  // 你可以在这里修改原始css
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
  // 如果条件被匹配，加载CSS并转换它
  if (css.includes('something I want to check')) {
    return css;
  }
  // 如果返回一个falsy值，CSS不会被加载
  return false
}
```

### `insertAt`

默认情况下，style-loader 将`<style>`元素附加到样式目标的末尾，这是页面的`<head>`标签，除非`insertInto`被指定。这将导致loader创建的CSS优先于目标中已经存在的CSS。要在目标的开头插入样式元素，将这个查询参数设置为'top'。

**webpack.config.js**
```js
{
  loader: 'style-loader',
  options: {
    insertAt: 'top'
  }
}
```

可以传递一个object来指定一个元素，一个新的`<style>`元素可以被添加到这个元素前面。

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

默认情况下，style-loader将`<style>`元素插入到页面的`<head>`标签中。如果您希望将标签插入到其他地方，那么您可以为该元素指定一个CSS选择器。如果您的目标是一个[IFrame](https://developer.mozilla.org/en-US/docs/Web/API/HTMLIFrameElement)，请确保您有足够的访问权限，那么样式将被注入到内容文档头部。您也可以将样式插入到[ShadowRoot](https://developer.mozilla.org/en-US/docs/Web/API/ShadowRoot)中。例如：

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
如果定义了，style-loader将重用一个单一的`<style>`元素，而不是为每个需要的模块 添加/删除 单个元素。

> **[info]**  
>
>这个选项在IE9中是默认的，它对页面上允许的样式标签数量有严格的限制。您可以使用singleton选项启用或禁用它。

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

启用/禁用 source map 加载

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

如果 convertToAbsoluteUrls 和 sourceMaps 都启用，那么相对 url 将在 css 注入页面之前，被转换为绝对 url。这解决了在启用 source map 时，相对资源无法加载的[问题](https://github.com/webpack/style-loader/pull/96)。您可以使用 convertToAbsoluteUrls 选项启用它。

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

<h2>维护者</h2>

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
