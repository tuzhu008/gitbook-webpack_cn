# module （模块）

该选项用来配置如何处理各种模块。配置的方式是对模块应用一系列的规则（Rule）。

```js
module.exports = {
        module: {
        rules: [
            {
                test: '',   
                use: []
            }
        ]
    }
}
```

## module.rules

类型：array

创建模块时，匹配请求的[规则](https://doc.webpack-china.org/configuration/module/#rule)数组。

```js
module.exports = {
    module: {
        rules: [
            {
                use:
            }
        ]
    }
}
```

# Rule

规则，能够修改模块的创建方式。能够对模块应用 loader，或者修改解析器\(parser\)。

每个规则分为三个部分：条件\(condition\)，结果\(result\)和嵌套规则\(nested rule\)。

### 条件

条件有两种输入值：

1. resource: 请求文件的绝对路径。它根据[resolve](/configuration/resolve.md)规则进行解析。
2. issuer: 被请求资源\(requested the resource\)的模块文件的绝对路径。是导入时的位置。

**例如:**从`app.js导入 './style.css'`，resource 是`/path/to/style.css`  `. issuer` 是`/path/to/app.js`。

在规则中，属性[`test`](https://doc.webpack-china.org/configuration/module/#rule-test),[`include`](https://doc.webpack-china.org/configuration/module/#rule-include),[`exclude`](https://doc.webpack-china.org/configuration/module/#rule-exclude)和[`resource`](https://doc.webpack-china.org/configuration/module/#rule-resource)对 resource 匹配，并且属性[`issuer`](https://doc.webpack-china.org/configuration/module/#rule-issuer)对 issuer 匹配。

当使用多个条件时，所有条件都匹配。

> \*\*\[warning\]\*\*
>
> 小心！resource 是文件的_解析_路径，这意味着符号链接的资源是真正的路径，_而不是_符号链接位置。在使用工具来符号链接包的时候（如`npm link`）比较好记，像`/node_modules/`等常见条件可能会不小心错过符号链接的文件。

### 结果

规则结果只在规则条件匹配时使用。

规则有两种输入值：

1. 应用的 loader：应用在 resource 上的 loader 数组。
2. Parser 选项：用于为模块创建解析器的选项对象。

这些属性会影响 loader：[`loader`](https://doc.webpack-china.org/configuration/module/#rule-loader)，[`options`](https://doc.webpack-china.org/configuration/module/#rule-options-rule-query)，[`use`](https://doc.webpack-china.org/configuration/module/#rule-use)。

也兼容这些属性：[`query`](https://doc.webpack-china.org/configuration/module/#rule-options-rule-query)，[`loaders`](https://doc.webpack-china.org/configuration/module/#rule-loaders)。

[`enforce`](https://doc.webpack-china.org/configuration/module/#rule-enforce)属性会影响 loader 种类。不论是普通的，前置的，后置的 loader。

[`parser`](https://doc.webpack-china.org/configuration/module/#rule-parser)属性会影响 parser 选项。

### 嵌套规则

可以使用属性[`rules`](https://doc.webpack-china.org/configuration/module/#rule-rules)和[`oneOf`](https://doc.webpack-china.org/configuration/module/#rule-oneof)指定嵌套规则。

这些规则用于在规则条件\(rule condition\)匹配时进行取值。

### `Rule.resource`

[`条件`](#条件)会匹配 resource。既可以提供`Rule.resource`选项，也可以使用快捷选项`Rule.test`，`Rule.exclude`和`Rule.include`。在[Rule`条件`](#rule条件)中查看详细。

### `Rule.test`

Rule.test 是 Rule.resource.test的简写。如果你提供了一个`Rule.test`选项，就不能再提供`Rule.resource`。

### `Rule.use`

应用于模块的[UseEntries](https://doc.webpack-china.org/configuration/module/#useentry)列表。每个入口\(entry\)指定使用一个 loader。

传递字符串（如：`use: [ "style-loader" ]`）是 loader 属性的简写方式（如：`use: [ { loader: "style-loader "} ]`）。

当然也可以为同一模块使用多个loader，并且我们还可以对每个loader进行设置：

```js
module.exports = {
    module: {
        rules: [
            {
                test: /\.css/,
                use: [
                    'style-loader',
                    {
                        loader: 'css-loader',
                        options: {
                          importLoaders: 1
                        }
                    }
                ]
            },
        ]
    }
}
```

> **\[info\] 注：**
>
> 对同一条件使用多个loader时，解析顺序为从**右**到**左，**从上到下，最后一个loader返回经过最终转换的模块。

### `Rule.options / Rule.query`

`Rule.options` 和` Rule.query`是`Rule.use: [ { options } ]`的简写。是用来对loader进行配置的。

```js
use: [
    {
        loader: 'css-loader',
        options: {
            importLoaders: 1
        }
    },
]
```

## Rule条件

条件可以是这些之一：

* 字符串：匹配输入必须以提供的字符串开始。是的。目录绝对路径或文件绝对路径。
* 正则表达式：test 输入值。
* 函数：调用输入的函数，必须返回一个真值\(truthy value\)以匹配。
* 条件数组：**至少**一个匹配条件。
* 对象：匹配所有属性。每个属性都有一个定义行为。

`{ test: Condition }`：匹配特定条件。一般是提供一个正则表达式或正则表达式的数组，但这不是强制的。

`{ include: Condition }`：匹配特定条件。一般是提供一个字符串或者字符串数组，但这不是强制的。

`{ exclude: Condition }`：排除特定条件。一般是提供一个字符串或字符串数组，但这不是强制的。

`{ and: [Condition] }`：必须匹配数组中的所有条件

`{ or: [Condition] }`：匹配数组中任何一个条件

`{ not: [Condition] }`：必须排除这个条件

**示例:**

```js
{
  test: /\.css$/,
  include: [
    path.resolve(__dirname, "app/styles"),
    path.resolve(__dirname, "vendor/styles")
  ]
}
```



