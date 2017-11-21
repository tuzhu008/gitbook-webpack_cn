# BannerPlugin

为每个 chunk 文件头部添加 banner。

```js
new webpack.BannerPlugin(banner)
// or
new webpack.BannerPlugin(options)
```

## `options`

类型：object

```js
{
  banner: string, // 其值为字符串，将作为注释存在
  raw: boolean, // 如果值为 true，将不会被包裹在注释的
  entryOnly: boolean, // 如果值为 true，将只在入口 chunks 文件中添加
  test: string | RegExp | Array,
  include: string | RegExp | Array,
  exclude: string | RegExp | Array,
}
```

## 占位符

从 webpack 2.5.0起， 在`banner`字符串中的占位符会被求值：

```js
new webpack.BannerPlugin({
  banner: "hash:[hash], chunkhash:[chunkhash], name:[name], filebase:[filebase], query:[query], file:[file]"
})
```



---

## 进一步阅读

* [banner-plugin-hashing test](https://github.com/webpack/webpack/blob/master/test/configCases/plugins/banner-plugin-hashing/webpack.config.js)



