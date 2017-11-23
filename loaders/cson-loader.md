# cson-loader

[![](https://img.shields.io/badge/Github-%E6%9F%A5%E7%9C%8B%E6%9B%B4%E5%A4%9A-brightgreen.svg)](https://github.com/awnist/cson-loader)

Uses [cson-safe](https://github.com/groupon/cson-safe) under the hood.

## 推荐用法

```js
{
	resolve: {
		extensions: ['.cson']
	},
	module: {
		loaders: [
			{ test: /\.cson$/, loader: "cson" }
		]
	}
}
```

然后，就像：

```js
var contents = require("config") // will load config.cson
```

## inline

```js
var contents = require("cson!./file.cson");
```



