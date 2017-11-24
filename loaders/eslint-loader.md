# eslint-loader [![Build Status](http://img.shields.io/travis/MoOx/eslint-loader.svg)](https://travis-ci.org/MoOx/eslint-loader)

[![](https://img.shields.io/badge/Github-%E6%9F%A5%E7%9C%8B%E6%9B%B4%E5%A4%9A-brightgreen.svg)](https://github.com/MoOx/eslint-loader)


> eslint loader for webpack

---

<a target='_blank' rel='nofollow' href='https://app.codesponsor.io/link/6RNUx3a3Vj2k5iApeppsc9L9/MoOx/eslint-loader'>  <img alt='Sponsor' width='888' height='68' src='https://app.codesponsor.io/embed/6RNUx3a3Vj2k5iApeppsc9L9/MoOx/eslint-loader.svg' /></a>

---

## 安装

```console
$ npm install eslint-loader --save-dev
```

> **[info]** 注：
>
> 您还需要从npm安装`eslint`，如果您还没有这样做的话:

```console
$ npm install eslint --save-dev
```

## 用法

在webpack配置中

```js
module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: "eslint-loader",
        options: {
          // eslint 选项 (如果有必要)
        }
      },
    ],
  },
  // ...
}
```

在使用转换loader(比如`babel-loader`)时，确保它们的顺序是正确的。
(从下到上，从右到左)。否则，文件将在被`babel-loader`处理后进行检查

```js
module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: [
          "babel-loader",
          "eslint-loader", //在被处理前检查
        ],
      },
    ],
  },
  // ...
}
```

为了安全起见，您可以使用`enforce: "pre"`部分来检查源文件，而不是修改
其他loader(比如`babel-loader`)

```js
module.exports = {
  // ...
  module: {
    rules: [
      {
        enforce: "pre",
        test: /\.js$/,
        exclude: /node_modules/,
        loader: "eslint-loader",
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: "babel-loader",
      },
    ],
  },
  // ...
}
```

### 选项

您可以使用标准的webpack [loader选项](//configuration/module/#useentry)来传递[eslint 选项](http://eslint.org/docs/developer-guide/nodejs-api#cliengine)。

> **[info]**
>
>请注意，您提供的配置选项将被传递给`CLIEngine`。这是与您在`package.json`或`.eslintrc`中指定的不同的一组选项。查看[eslint 文档](http://eslint.org/docs/developer-guide/nodejs-api#cliengine) 获取更多细节。


#### `fix` (默认值: false)

这个选项将启用
[ESLint 自动修复特性](http://eslint.org/docs/user-guide/command-line-interface#fix).

> **[warning]** 警告：
>
> 这个选项可能会导致webpack进入一个无限的构建循环，如果有些问题是无法正确解决的。

#### `cache` (默认值: false)

这个选项将允许将linting结果缓存到一个文件中。
这对于在进行完整构建时减少linting时间非常有用。

这可以是一个布尔值，也可以是缓存目录路径(例如:`'./.eslint-loader-cache'`)。

如果`cache: true`被使用，则将缓存文件写入`./node_modules/.cache`目录。
这是推荐的用法。

#### `formatter` (默认值: eslint stylish formatter)

loader接受一个有一个参数的函数:eslint消息(对象)的数组。
函数必须以字符串的形式返回输出。
您可以使用正式的eslint格式器。

```js
module.exports = {
  entry: "...",
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: "eslint-loader",
        options: {
          // 几个例子 !

          // 默认值
          formatter: require("eslint/lib/formatters/stylish"),

          // 社区的 格式器（formatter）
          formatter: require("eslint-friendly-formatter"),

          // 自定义 formatter
          formatter: function(results) {
            // `results` 格式在这里是可用的
            // http://eslint.org/docs/developer-guide/nodejs-api.html#executeonfiles()

            // 你应该返回一个字符串
            // 请勿直接使用 console.*() !
            return "OUTPUT"
          }
        }
      },
    ],
  },
}
```

#### `eslintPath` (默认值: "eslint")

`eslint`实例的路径，将被用来linting。

```js
module.exports = {
  entry: "...",
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: "eslint-loader",
        options: {
          eslintPath: path.join(__dirname, "reusable-eslint-rules.js"),
        }
      },
    ],
  },
  }
```

#### 错误和警告


**默认情况下， loader将根据eslint错误/警告计数自动调整错误报告。**
您仍然可以使用`emitError`或`emitWarning`选项来强制这种行为:

##### `emitError` (default: `false`)

如果这个选项被设置为`true`，loader将总是返回错误。

```js
module.exports = {
  entry: "...",
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: "eslint-loader",
        options: {
          emitError: true,
        }
      },
    ],
  },
}
```

##### `emitWarning` (默认值: `false`)

如果该选项被设置为`true`，loader将总是返回警告。如果您正在使用模块热替换，您可能希望在开发中启用此功能，否则在出现eslint错误时将跳过更新。

#### `quiet` (default: `false`)

如果这个选项被设置为`true`，loader只会处理和报告错误，而会忽略警告

```js
module.exports = {
  entry: "...",
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: "eslint-loader",
        options: {
          quiet: true,
        }
      },
    ],
  },
}
```

##### `failOnWarning` (默认值: `false`)

如果有任何eslint警告，loader将导致模块构建失败。

```js
module.exports = {
  entry: "...",
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: "eslint-loader",
        options: {
          failOnWarning: true,
        }
      },
    ],
  },
}
```

##### `failOnError` (default: `false`)

如果存在任何eslint错误，loader将导致模块构建失败。

```js
module.exports = {
  entry: "...",
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: "eslint-loader",
        options: {
          failOnError: true,
        }
      },
    ],
  },
}
```

##### `outputReport` (default: `false`)

将错误的输出写入一个文件，例如一个用于报告Jenkins CI的checkstyle xml文件

`filePath`相对于webpack配置:`output.path`。
您可以为输出文件传入一个不同的格式化程序（formatter），如果没有使用默认/配置的格式化程序，则可以使用该文件。

```js
module.exports = {
  entry: "...",
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: "eslint-loader",
        options: {
          outputReport: {
            filePath: 'checkstyle.xml',
            formatter: require('eslint/lib/formatters/checkstyle')
          }
        }
      },
    ],
  },
}
```


## 陷阱

### NoErrorsPlugin

`NoErrorsPlugin`会防止webpack将任何东西输出到一个bundle中。所以即使ESLint警告
将构建失败。无论什么错误设置被用于`eslint-loader`。

因此，如果您想在使用`WebpackDevServer`的开发过程中在控制台中看到ESLint警告
请从从webpack配置中删除`NoErrorsPlugin`。


### 定义 `configFile` 或者 使用 `eslint -c path/.eslintrc`

请记住，当您定义`configFile`时，`eslint`并不会自动地在文件的目录中查找`.eslintrc`文件。
更多的信息可以在官方的eslint文档中找到 [_Using Configuration Files_](http://eslint.org/docs/user-guide/configuring#using-configuration-files).
参见 [#129](https://github.com/MoOx/eslint-loader/issues/129).

---

## [Changelog](CHANGELOG.md)

## [License](LICENSE)
