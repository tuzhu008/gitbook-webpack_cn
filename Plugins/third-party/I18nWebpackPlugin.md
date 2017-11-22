# I18nWebpackPlugin

[![](https://img.shields.io/badge/Github-%E6%9F%A5%E7%9C%8B%E6%9B%B4%E5%A4%9A-brightgreen.svg)](https://github.com/webpack-contrib/i18n-webpack-plugin)

将本地化嵌入到你的webpack中。

## 安装

```bash
npm i -D i18n-webpack-plugin
```

## 用法

此插件会创建包含译文的 bundle。所以您可以将翻译后的 bundle 提供给客户端。

```js
new I18nPlugin(languageConfig, optionsObj)
```

## 配置

### `languageConfig`

### `optionsObj`

类型：object

有以下选项：

* `functionName`：默认值`__`, 你可以更改为其他函数名。

* `failOnMissing`：默认值为`false`，找不到映射文本\(mapping text\)时会给出一个警告信息，如果设置为`true`，则会给出一个错误信息。
* `hideMessage`：默认值为`false`，将会显示警告/错误信息。如果设置为`true`，警告/错误信息将会被隐藏。

## 示例



