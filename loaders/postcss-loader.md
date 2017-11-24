[![npm][npm]][npm-url]
[![node][node]][node-url]
[![deps][deps]][deps-url]
[![tests][tests]][tests-url]
[![coverage][cover]][cover-url]
[![chat][chat]][chat-url]

<div align="center">
  <img width="180" height="180" hspace="10"
    alt="PostCSS Logo"
    src="http://postcss.github.io/postcss/logo.svg">
  <a href="https://github.com/webpack/webpack">
    <img width="200" height="200" hspace="10"
      src="https://webpack.js.org/assets/icon-square-big.svg">
  </a>
  <div align="center">
    <a href="https://evilmartians.com/?utm_source=postcss">
      <img src="https://evilmartians.com/badges/sponsored-by-evil-martians.svg"
        alt="Sponsored by Evil Martians" width="236" height="54" vspace="10">
    </a>
  </div>
  <h1>PostCSS Loader</h1>
  
  <p> <a href="http://webpack.js.org/">webpack</a> loader，使用 <a href="http://postcss.org/">PostCSS</a> 来处理CSS</p>
  <a href="https://github.com/postcss/postcss-loader"><img src="https://img.shields.io/badge/Github-%E6%9F%A5%E7%9C%8B%E6%9B%B4%E5%A4%9A-brightgreen.svg" alt='postCss'></a>
</div>


<h2 align="center">安装</h2>


```bash
npm i -D postcss-loader
```

<h2 align="center">用法</h2>


### `配置`

**postcss.config.js**
```js
module.exports = {
  parser: 'sugarss',
  plugins: {
    'postcss-import': {},
    'postcss-cssnext': {},
    'cssnano': {}
  }
}
```

你可以在[这里](https://github.com/michael-ciniawsky/postcss-load-config)查看更多关于常见的 PostCSS 配置.

### `配置级联（config Cascade）`

你可以在不同的目录使用不同的  `postcss.config.js` 文件 。
配置查找从`path.dirname(file)`开始，并沿文件树向上走，直到找到一个配置文件。

```
|– components
| |– component
| | |– index.js
| | |– index.png
| | |– style.css (1)
| | |– postcss.config.js (1)
| |– component
| | |– index.js
| | |– image.png
| | |– style.css (2)
|
|– postcss.config.js (1 && 2 (recommended))
|– webpack.config.js
|
|– package.json
```

设置好`postcss.config.js`之后，在`webpack.config.js`中添加`postcss-loader`。您可以独立使用它，也可以与`css-loader`(推荐)结合使用。在`css-loader`和`style-loader`之后，但是在像如`sass|less|stylus-loader`这样的预处理器之前使用它。

**webpack.config.js**
```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [ 'style-loader', 'postcss-loader' ]
      }
    ]
  }
}
```

> **[warning]** 警告
>
> 当 `postcss-loader` 被独立使用时（没有`css-loader`），不要在CSS中使用`@import`，因为这样会导致导致相当臃肿的bundle

**webpack.config.js (推荐)**
```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          'style-loader',
          { loader: 'css-loader', options: { importLoaders: 1 } },
          'postcss-loader'
        ]
      }
    ]
  }
}
```

<h2 align="center">选项</h2>

|名称|类型|默认值|描述|
|:--:|:--:|:-----:|:----------|
|[`exec`](#exec)|`{Boolean}`|`undefined`|Enable PostCSS Parser support in `CSS-in-JS`|
|[`parser`](#syntaxes)|`{String\|Object}`|`undefined`|Set PostCSS Parser|
|[`syntax`](#syntaxes)|`{String\|Object}`|`undefined`|Set PostCSS Syntax|
|[`stringifier`](#syntaxes)|`{String\|Object}`|`undefined`|Set PostCSS Stringifier|
|[`config`](#config)|`{Object}`|`undefined`|Set `postcss.config.js` config path && `ctx`|
|[`plugins`](#plugins)|`{Array\|Function}`|`[]`|Set PostCSS Plugins|
|[`sourceMap`](#sourceMap)|`{String\|Boolean}`|`false`|Enable Source Maps|

### `Exec`

If you use JS styles without the [`postcss-js`][postcss-js] parser, add the `exec` option.

```js
{
  test: /\.style.js$/,
  use: [
    'style-loader',
    { loader: 'css-loader', options: { importLoaders: 1 } },
    { loader: 'postcss-loader', options: { parser: 'sugarss', exec: true } }
  ]
}
```

### `Config`

|Name|Type|Default|Description|
|:--:|:--:|:-----:|:----------|
|[`path`](#path)|`{String}`|`undefined`|PostCSS Config Path|
|[`context`](#context)|`{Object}`|`undefined`|PostCSS Config Context|

#### `Path`

You can manually specify the path to search for your config (`postcss.config.js`) with the `config.path` option. This is needed if you store your config  in a separate e.g `./config || ./.config` folder.

> ⚠️  Otherwise it is **unnecessary** to set this option and is **not** recommended

**webpack.config.js**
```js
{
  loader: 'postcss-loader',
  options: {
    config: {
      path: 'path/to/postcss.config.js'
    }
  }
}
```

#### `Context (ctx)`

|Name|Type|Default|Description|
|:--:|:--:|:-----:|:----------|
|`env`|`{String}`|`'development'`|`process.env.NODE_ENV`|
|`file`|`{Object}`|`loader.resourcePath`|`extname`, `dirname`, `basename`|
|`options`|`{Object}`|`{}`|Options|

`postcss-loader` exposes context `ctx` to the config file, making your `postcss.config.js` dynamic, so can use it to do some real magic ✨

**postcss.config.js**
```js
module.exports = ({ file, options, env }) => ({
  parser: file.extname === '.sss' ? 'sugarss' : false,
  plugins: {
    'postcss-import': { root: file.dirname },
    'postcss-cssnext': options.cssnext ? options.cssnext : false,
    'autoprefixer': env == 'production' ? options.autoprefixer : false,
    'cssnano': env === 'production' ? options.cssnano : false
  }
})
```

**webpack.config.js**
```js
{
  loader: 'postcss-loader',
  options: {
    config: {
      ctx: {
        cssnext: {...options},
        cssnano: {...options},
        autoprefixer: {...options}
      }
    }
  }
}
```

### `Plugins`

**webpack.config.js**
```js
{
  loader: 'postcss-loader',
  options: {
    ident: 'postcss',
    plugins: (loader) => [
      require('postcss-import')({ root: loader.resourcePath }),
      require('postcss-cssnext')(),
      require('autoprefixer')(),
      require('cssnano')()
    ]
  }
}
```

> ⚠️  webpack requires an identifier (`ident`) in `options` when `{Function}/require` is used (Complex Options). The `ident` can be freely named as long as it is unique. It's recommended to name it (`ident: 'postcss'`)

### `Syntaxes`

|Name|Type|Default|Description|
|:--:|:--:|:-----:|:----------|
|[`parser`](#parser)|`{String\|Function}`|`undefined`|Custom PostCSS Parser|
|[`syntax`](#syntax)|`{String\|Function}`|`undefined`|Custom PostCSS Syntax|
|[`stringifier`](#stringifier)|`{String\|Function}`|`undefined`|Custom PostCSS Stringifier|

#### `Parser`

**webpack.config.js**
```js
{
  test: /\.sss$/,
  use: [
    ...,
    { loader: 'postcss-loader', options: { parser: 'sugarss' } }
  ]
}
```

#### `Syntax`

**webpack.config.js**
```js
{
  test: /\.css$/,
  use: [
    ...,
    { loader: 'postcss-loader', options: { syntax: 'sugarss' } }
  ]
}
```

#### `Stringifier`

**webpack.config.js**
```js
{
  test: /\.css$/,
  use: [
    ...,
    { loader: 'postcss-loader', options: { stringifier: 'midas' } }
  ]
}
```

### `SourceMap`

Enables source map support, `postcss-loader` will use the previous source map given by other loaders and update it accordingly, if no previous loader is applied before `postcss-loader`, the loader will generate a source map for you.

> :warning: If a previous loader like e.g `sass-loader` is applied and it's `sourceMap` option is set, but the `sourceMap` option in `postcss-loader` is omitted, previous source maps will be discarded by `postcss-loader` **entirely**.

**webpack.config.js**
```js
{
  test: /\.css/,
  use: [
    { loader: 'style-loader', options: { sourceMap: true } },
    { loader: 'css-loader', options: { sourceMap: true } },
    { loader: 'postcss-loader', options: { sourceMap: true } },
    { loader: 'sass-loader', options: { sourceMap: true } }
  ]
}
```

#### `'inline'`

You can set the `sourceMap: 'inline'` option to inline the source map
within the CSS directly as an annotation comment.

**webpack.config.js**
```js
{
  loader: 'postcss-loader',
  options: {
    sourceMap: 'inline'
  }
}
```

```css
.class { color: red; }

/*# sourceMappingURL=data:application/json;base64, ... */
```

<h2 align="center">Examples</h2>

### `Stylelint`

**webpack.config.js**
```js
{
  test: /\.css$/,
  use: [
    'style-loader',
    'css-loader',
    {
      loader: 'postcss-loader',
      options: {
        ident: 'postcss',
        plugins: [
          require('postcss-import')(),
          require('stylelint')(),
          ...,
        ]
      }
    }
  ]
}
```

### `CSS Modules`

This loader [cannot be used] with [CSS Modules] out of the box due
to the way `css-loader` processes file imports. To make them work properly,
either add the css-loader’s [`importLoaders`] option.

**webpack.config.js**
```js
{
  test: /\.css$/,
  use: [
    'style-loader',
    { loader: 'css-loader', options: { modules: true, importLoaders: 1 } },
    'postcss-loader'
  ]
}
```

or use [postcss-modules] instead of `css-loader`.

[`importLoaders`]: https://github.com/webpack-contrib/css-loader#importloaders
[cannot be used]: https://github.com/webpack/css-loader/issues/137
[CSS Modules]: https://github.com/webpack/css-loader#css-modules
[postcss-modules]: https://github.com/outpunk/postcss-modules

### `CSS-in-JS`

If you want to process styles written in JavaScript, use the [postcss-js] parser.

[postcss-js]: https://github.com/postcss/postcss-js

```js
{
  test: /\.style.js$/,
  use: [
    'style-loader',
    { loader: 'css-loader', options: { importLoaders: 2 } },
    { loader: 'postcss-loader', options: { parser: 'postcss-js' } },
    'babel-loader'
  ]
}
```

As result you will be able to write styles in the following way

```js
import colors from './styles/colors'

export default {
    '.menu': {
      color: colors.main,
      height: 25,
      '&_link': {
      color: 'white'
    }
  }
}
```

> :warning: If you are using Babel you need to do the following in order for the setup to work

> 1. Add [babel-plugin-add-module-exports] to your configuration
> 2. You need to have only one **default** export per style module

[babel-plugin-add-module-exports]: https://github.com/59naga/babel-plugin-add-module-exports

### [Extract CSS][ExtractPlugin]

[ExtractPlugin]: https://github.com/webpack-contrib/extract-text-webpack-plugin

**webpack.config.js**
```js
const ExtractTextPlugin = require('extract-text-webpack-plugin')

module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ExtractTextPlugin.extract({
          fallback: 'style-loader',
          use: [
            { loader: 'css-loader', options: { importLoaders: 1 } },
            'postcss-loader'
          ]
        })
      }
    ]
  },
  plugins: [
    new ExtractTextPlugin('[name].css')
  ]
}
```

<h2 align="center">Maintainers</h2>

<table>
  <tbody>
    <tr>
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


[npm]: https://img.shields.io/npm/v/postcss-loader.svg
[npm-url]: https://npmjs.com/package/postcss-loader

[node]: https://img.shields.io/node/v/postcss-loader.svg
[node-url]: https://nodejs.org

[deps]: https://david-dm.org/postcss/postcss-loader.svg
[deps-url]: https://david-dm.org/postcss/postcss-loader

[tests]: http://img.shields.io/travis/postcss/postcss-loader.svg
[tests-url]: https://travis-ci.org/postcss/postcss-loader

[cover]: https://coveralls.io/repos/github/postcss/postcss-loader/badge.svg
[cover-url]: https://coveralls.io/github/postcss/postcss-loader

[chat]: https://badges.gitter.im/postcss/postcss.svg
[chat-url]: https://gitter.im/postcss/postcss

