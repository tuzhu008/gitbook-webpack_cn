# I18nWebpackPlugin

[![](https://img.shields.io/badge/Github-查看更多-brightgreen.svg)](https://github.com/webpack-contrib/i18n-webpack-plugin)

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

这个例子使用了`I18nPlugin`和多编译器的特性。

`webpack.config.js`导出了应该被编译的所有配置组合的数组。在本例中，使用了两个不同参数`I18nPlugin`。

I18nPlugin用一个常量字符串替换了i18n函数`__(...)`的每一个事件。即。`__("Hello World")`和“Hello World”职责。“喂沿条”。例如`__("Hello World")`，替换后为`"Hallo Welt"`

### 例如：

```js
console.log(__("Hello World"));
console.log(__("Missing Text"));
```

### webpack.config.js

```js
var path = require("path");
var I18nPlugin = require("i18n-webpack-plugin");
var languages = {
	"en": null,
	"de": require("./de.json")
};
module.exports = Object.keys(languages).map(function(language) {
	return {
		name: language,
		entry: "./example",
		output: {
			path: path.join(__dirname, "js"),
			filename: language + ".output.js"
		},
		plugins: [
			new I18nPlugin(
				languages[language]
			)
		]
	};
});
```

### de.json

```js
{
	"Hello World": "Hallo Welt"
}
```



