# Module （模块）

该选项用来配置如何处理各种模块。配置的方式是对模块应用一系列的规则（Rule）。

```js
module.exports = {
        module: {
        rules: '', // 规则
        noparse: '' // 排除匹配的文件，不进行解析
    }
}
```

## module.rules

类型：array

创建模块时，匹配请求的[规则](#rule)数组。

```js
module.exports = {
    module: {
        rules: [
            {
                rules: [],
                resource: '',
                test: '',
                include: '',
                exclude: '',
                resourceQuery: '',
                use: '',
                issuer: '',
                oneOf: '',
                options: '',
                parser: '',

            }
        ]
    }
}
```

## Rule

规则，能够修改模块的创建方式。能够对模块应用 loader，或者修改解析器\(parser\)。

每个规则分为三个部分：条件\(condition\)，结果\(result\)和嵌套规则\(nested rule\)。

### 条件

条件有两种输入值：

1. resource: 请求文件的绝对路径。它根据[resolve](/configuration/resolve.md)规则进行解析。
2. issuer: 被请求资源\(requested the resource\)的模块文件的绝对路径。是导入时的位置。

**例如:**从`app.js导入 './style.css'`，resource 是`/path/to/style.css`  `. issuer` 是`/path/to/app.js`。

在规则中，属性[`test`](#ruletest),[`include`](#ruleinclude),[`exclude`](#ruleexclude)和[`resource`](#ruleresource)对 resource 匹配，并且属性[`issuer`](#ruleissuer)对 issuer 匹配。

当使用多个条件时，所有条件都匹配。

> \*\*\[warning\]\*\*
>
> 小心！resource 是文件的_解析_路径，这意味着符号链接的资源是真正的路径，_而不是_符号链接位置。在使用工具来符号链接包的时候（如`npm link`）比较好记，像`/node_modules/`等常见条件可能会不小心错过符号链接的文件。

### 结果

规则结果只在规则条件匹配时使用。

规则有两种输入值：

1. 应用的 loader：应用在 resource 上的 loader 数组。
2. Parser 选项：用于为模块创建解析器的选项对象。

这些属性会影响 loader：[`options`](https://doc.webpack-china.org/configuration/module/#rule-options-rule-query)，[`use`](https://doc.webpack-china.org/configuration/module/#rule-use)。

也兼容这些属性：[`query`](#ruleoptions--rulequery)。

[`enforce`](#ruleenforce)属性会影响 loader 种类。不论是普通的，前置的，后置的 loader。

[`parser`](#ruleparser)属性会影响 parser 选项。

### 嵌套规则

可以使用属性[`rules`](#rulerules)和[`oneOf`](#ruleoneof)指定嵌套规则。

这些规则用于在规则条件\(rule condition\)匹配时进行取值。

### `Rule.rules`

[`规则`](#rule)数组，当规则匹配时使用。

```js
module.exports = {
    module: {
        rules: [
            {
                rules: []
            }
        ]
    }
}
```

### `Rule.resource`

[`条件`](#条件)会匹配 resource。既可以提供`Rule.resource`选项，也可以使用快捷选项`Rule.test`，`Rule.exclude`和`Rule.include`。在[Rule`条件`](#rule条件)中查看详细。

### `Rule.test`

`Rule.test`是`Rule.resource.test`的简写。如果你提供了一个`Rule.test`选项，就不能再提供`Rule.resource`。

### `Rule.include`

`Rule.include`是`Rule.resource.include`的简写。如果你提供了`Rule.include`选项，就不能再提供`Rule.resource`。

### `Rule.exclude`

`Rule.exclude`是`Rule.resource.exclude`的简写。如果你提供了`Rule.exclude`选项，就不能再提供`Rule.resource`。

### `Rule.resourceQuery`

与资源查询相匹配的[条件](#条件)。这个选项用于`test`对应的请求字符串的查询部分\(也就是从问号开始\)。如果你要从`import Foo from './foo.css?inline'`，以下条件将匹配:

```js
{
  test: /.css$/,
  resourceQuery: /inline/,
  use: 'url-loader'
}
```

### `Rule.use`

应用于模块的[UseEntries](#use-entries)列表。每个入口\(entry\)指定使用一个 loader。

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

`Rule.options` 和`Rule.query`是`Rule.use: [ { options } ]`的简写。是用来对loader进行配置的。

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

### 

### `Rule.oneOf`

规则数组，当匹配规则时，只会使用匹配到的第一个规则。

```js
{
  test: /.css$/,
  oneOf: [
    {
      resourceQuery: /inline/, // foo.css?inline
      use: 'url-loader'
    },
    {
      resourceQuery: /external/, // foo.css?external
      use: 'file-loader'
    }
  ]
}
```

### `Rule.issuer`

与发出请求的模块相匹配的条件。在下面的例子中，`a.js`的`issuer`，请求将是`index.js`文件的路径。

**index.js**

```js
import A from './a.js'
```

该选项可用于将loader应用于特定模块或模块集的依赖项。

### `Rule.enforce`

用于指定 loader 种类。没有值表示是普通 loader。

可能值：

| 取值 | 描述 |
| :--- | :--- |
| pre | 前置loader |
| post | 后置loader |
| 缺省 | 普通loader |

loader的类型有四种：`前置, 行内, 普通, 后置`。所有 loader 都按这个顺序排序，并按此顺序使用。

"行内 loader"，loader 被应用在 import/require 行内。

```js
import Styles from 'style-loader!css-loader?modules!./styles.css';
```

所有普通 loader 可以通过在请求中加上`!`前缀来忽略（覆盖）。

所有普通和前置 loader 可以通过在请求中加上`-!`前缀来忽略（覆盖）。

所有普通，后置和前置 loader 可以通过在请求中加上`!!`前缀来忽略（覆盖）。

不应该使用行内 loader 和`!`前缀，因为它们是非标准的。它们可在由 loader 生成的代码中使用。

### `Rule.parser`

解析选项对象。所有应用的解析选项都将合并。

解析器\(parser\)可以查阅这些选项，并相应地禁用或重新配置。大多数默认插件，会如下解释值：

* 将选项设置为`false`，将禁用解析器。

* 将选项设置为`true`，或不修改将其保留为`undefined`，可以启用解析器。

然而，一些解析器\(parser\)插件可能不光只接收一个布尔值。例如，内部的`NodeStuffPlugin`，可以接收一个对象，而不是`true`

，来为特定的规则添加额外的选项。

**示例（默认插件的解析器选项）**：

```js
parser: {
  amd: false, // 禁用 AMD
  commonjs: false, // 禁用 CommonJS
  system: false, // 禁用 SystemJS
  harmony: false, // 禁用 ES2015 Harmony import/export
  requireInclude: false, // 禁用 require.include
  requireEnsure: false, // 禁用 require.ensure
  requireContext: false, // 禁用 require.context
  browserify: false, // 禁用特殊处理的 browserify bundle
  requireJs: false, // 禁用 requirejs.*
  node: false, // 禁用 __dirname, __filename, module, require.extensions, require.main 等。
  node: {...} // 在模块级别(module level)上重新配置 node 层(layer)
}
```

### `UseEntry` {#use-entries}

类型：object

必须有一个`loader`属性是字符串。它使用 loader 解析选项（[resolveLoader](/configuration/resolve#resolveloader)），相对于配置中的[`context`](/configuration/context.md)来解析。

可以有一个`options`属性为字符串或对象。值可以传递到 loader 中，将其理解为 loader 选项。

由于兼容性原因，也可能有`query`属性，它是`options`属性的别名。使用`options`属性替代。

**示例：**

```js
{
  loader: "css-loader",
  options: {
    modules: true
  }
}
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

## `module.noParse`

类型：RegExp \| \[RegExp\] \| function

防止 webpack 解析那些任何与给定正则表达式相匹配的文件。忽略的文件中**不应该含有**`import`，`require`，`define`的调用，或任何其他导入机制。忽略大型的 library 可以提高构建性能。

```js
// 从 webpack 3.0.0 开始
noParse: function(content) {
  return /jquery|lodash/.test(content); //返回值为布尔值
}
```



