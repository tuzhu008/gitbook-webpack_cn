# i18n loader for webpack

[![](https://img.shields.io/badge/Github-查看更多-brightgreen.svg)](https://github.com/webpack-contrib/i18n-loader)

## 用法

### ./colors.json

```javascript
{
    "red": "red",
    "green": "green",
    "blue": "blue"
}
```

### ./de-de.colors.json

```javascript
{
    "red": "rot",
    "green": "gr�n"
}
```

### 调用它

```javascript
// 假设我们的语言环境是 "de-de-berlin"
var locale = require("i18n!./colors.json");

// 等待就绪，这只需要为web应用程序的所有地区 require 一次，因为同一语言的所有语言环境都被合并到一个chuck中
locale(function() {
    console.log(locale.red); // prints rot
    console.log(locale.blue); // prints blue
});
```

### 选项

如果想要加载它们一次，而不是想要同步使用它们，那么您应该告诉loader关于所有地区的情况

```javascript
{
    "i18n": {
        "locales": [
            "de",
            "de-de",
            "fr"
        ],
        // "bundleTogether": false
        // 这可以禁用地区bundling
    }
}
```

### alternative calls选择电话

```javascript
require("i18n/choose!./file.js"); // 根据地区选择文件
                    // 但它不会合并对象
require("i18n/concat!./file.js"); // 连结所有合适的地区
require("i18n/merge!./file.js"); // 合并生成的对象
                    // ./file.js 在编译时执行
require("i18n!./file.json") == require("i18n/merge!json!./file.json")
```

如果想在node中使用它，请别忘了 polyfill `require`
参见 `webpack` 文档.

## License

MIT \([http://www.opensource.org/licenses/mit-license.php](http://www.opensource.org/licenses/mit-license.php)\)

