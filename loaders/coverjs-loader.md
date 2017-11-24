# coverjs loader for webpack

[![](https://img.shields.io/badge/Github-%E6%9F%A5%E7%9C%8B%E6%9B%B4%E5%A4%9A-brightgreen.svg)](https://github.com/webpack-contrib/coverjs-loader)

## 用法

``` javascript
webpack-dev-server "mocha!./cover-my-client-tests.js" --options webpackOptions.js
```

``` javascript
// webpackOptions.js
module.exports = {
	// webpack选项
	output: "bundle.js",
	publicPrefix: "/",
	debug: true,
	includeFilenames: true,
	watch: true,

	// the coverjs loader binding
	postLoaders: [{
		test: "", // 每个文件
		exclude: [
			"node_modules.chai",
			"node_modules.coverjs-loader",
			"node_modules.webpack.buildin"
		],
		loader: "coverjs-loader"
	}]
}
```

``` javascript
// cover-my-client-tests.js
require("./my-client-tests");

after(function() {
	require("cover-loader").reportHtml();
});
```

参见 [the-big-test](https://github.com/webpack/the-big-test) 获取示例。

你不需要将它与mocha loader相结合，它是独立的。因此，如果你想要覆盖一个正常的app用法，你可以这样做。`reportHtml`函数只是将输出附加到body上。


## License

MIT (http://www.opensource.org/licenses/mit-license.php)