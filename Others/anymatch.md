
anymatch [![Build Status](https://travis-ci.org/es128/anymatch.svg?branch=master)](https://travis-ci.org/es128/anymatch) [![Coverage Status](https://img.shields.io/coveralls/es128/anymatch.svg?branch=master)](https://coveralls.io/r/es128/anymatch?branch=master)
======

anymatch是一个Javascript模块。用来匹配一个字符串与正则表达式、glob、字符串、或者将字符串作为参数的并返回true或falsy值的函数。matcher也可以是任何或所有这些的数组。对允许一个非常灵活的用户定义配置来定义文件路径之类的东西是很有用的。

[![NPM](https://nodei.co/npm/anymatch.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/anymatch/)
[![NPM](https://nodei.co/npm-dl/anymatch.png?height=3&months=9)](https://nodei.co/npm-dl/anymatch/)

用法
-----
```sh
npm install anymatch --save
```

#### anymatch (matchers, testString, [returnIndex], [startIndex], [endIndex])
* __matchers__: (_Array|String|RegExp|Function_)
字符串被直接匹配，字符串带有glob模式，正则表达式测试，或者为一个函数。这个函数将测试字符串（testString）作为参数，如果匹配，或者是任何数字的数组，这些类型的混合就返回一个真值。
* __testString__: (_String|Array_) 如果
是一个数组，数组的第一个元素将被用作非函数marcher的`testString`，而整个数组将被应用作为函数mathcer的参数。
* __returnIndex__: (_Boolean [optional]_)如果是true，则返回测试字符串(testString)匹配的第一个matcher的数组索引，未匹配到，则返回-1，而不是一个布尔值。
* __startIndex, endIndex__: (_Integer [optional]_) 可用于在提供的matcher数组中定义一个子集来进行测试。适用于绑定的matcher函数(见下面)。当使用`returnIndex = true`保存原始索引时。行为与`Array.prototype.slice`相同。(也就是说，包括数组成员，但不包括endIndex，前闭后开区间)。

```js
var anymatch = require('anymatch');

var matchers = [
	'path/to/file.js',
	'path/anyjs/**/*.js',
	/foo\.js$/,
	function (string) {
		return string.indexOf('bar') !== -1 && string.length > 10
	}
];

anymatch(matchers, 'path/to/file.js'); // true
anymatch(matchers, 'path/anyjs/baz.js'); // true
anymatch(matchers, 'path/to/foo.js'); // true
anymatch(matchers, 'path/to/bar.js'); // true
anymatch(matchers, 'bar.js'); // false

// returnIndex = true
anymatch(matchers, 'foo.js', true); // 2
anymatch(matchers, 'path/anyjs/foo.js', true); // 1

// skip matchers
anymatch(matchers, 'path/to/file.js', false, 1); // false
anymatch(matchers, 'path/anyjs/foo.js', true, 2, 3); // 2
anymatch(matchers, 'path/to/bar.js', true, 0, 3); // -1

// 使用 globs 来匹配目录和他们的children
anymatch('node_modules', 'node_modules'); // true
anymatch('node_modules', 'node_modules/somelib/index.js'); // false
anymatch('node_modules/**', 'node_modules/somelib/index.js'); // true
anymatch('node_modules/**', '/absolute/path/to/node_modules/somelib/index.js'); // false
anymatch('**/node_modules/**', '/absolute/path/to/node_modules/somelib/index.js'); // true
```

#### anymatch (matchers)
你也可以只通过你的matcher(s)来得到一个curried的函数，它已经被绑定到提供的matching标准。这可以作为一个`Array.prototype.filter`的回调。

```js
var matcher = anymatch(matchers);

matcher('path/to/file.js'); // true
matcher('path/anyjs/baz.js', true); // 1
matcher('path/anyjs/baz.js', true, 2); // -1

['foo.js', 'bar.js'].filter(matcher); // ['foo.js']
```

Change Log
----------
[请参阅GitHub上的发布说明页面](https://github.com/es128/anymatch/releases)

NOTE: As of v1.2.0, anymatch uses [micromatch](https://github.com/jonschlinkert/micromatch)
for glob pattern matching. The glob matching behavior should be functionally
equivalent to the commonly used [minimatch](https://github.com/isaacs/minimatch)
library (aside from some fixed bugs and greater performance), so a major
version bump wasn't merited. Issues with glob pattern matching should be
reported directly to the [micromatch issue tracker](https://github.com/jonschlinkert/micromatch/issues).

License
-------
[ISC](https://raw.github.com/es128/anymatch/master/LICENSE)


