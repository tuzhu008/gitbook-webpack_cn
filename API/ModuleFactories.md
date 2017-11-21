# Module Factories

?&gt; Lead in...

## `NormalModuleFactory`

`before-resolve(data)` 瀑布式异步（waterfall）

在工厂开始解决（resolving）之前。 `data` 对象有这些属性：

* `context`：用于解析的目录的绝对路径。
* `request`：表达式的请求。

允许插件修改对象或将一个新的类似对象传递给回调。

`after-resolve(data)` 瀑布式异步（waterfall）

在工厂已经解决请求之后。`data` 对象有这些属性：

* `request`:  已解决的请求。它充当普通模块的标识符。
* `userRequest`:  用户输入的请求。它已经解决了，但不包含前置或后置loader。
* `rawRequest`: 未解决的请求。
* `loaders`:  已解决loader数组。这将传递给普通模块，并将执行它们。
* `resource`: 资源。它将由普通模块加载。
* `parser`: 将由普通模块使用的解析器。

## `ContextModuleFactory`

`before-resolve(data)` async waterfall

?&gt; Add documentation.

`after-resolve(data)` async waterfall

?&gt; Add documentation.

`alternatives(options: Array)` async waterfall

?&gt; Add documentation.

