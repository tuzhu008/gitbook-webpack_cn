# Hot Module Replacement（热模块替换）

如果已经通过[`HotModuleReplacementPlugin`](/Plugins/HotModuleReplacementPlugin.md)启用了[模块热替换\(Hot Module Replacement\)](https://doc.webpack-china.org/concepts/hot-module-replacement)，则它的接口将被暴露在[`module.hot`属性](https://doc.webpack-china.org/api/module-variables#module-hot-webpack-specific-)

下面。通常，用户先要检查这个接口是否可访问，然后再开始使用它。举个例子，你可以这样`accept`一个更新的模块：

```js
if (module.hot) {
  module.hot.accept('./library.js', function() {
    // 使用更新过的 library 模块执行某些操作...
  })
}
```

支持以下方法……

## `module.hot`



### `accept`

接受\(accept\)给定`依赖模块`的更新，并触发一个`回调函数`来对这些更新做出响应。

```js
module.hot.accept(
  dependencies, // 可以是一个字符串或字符串数组
  callback // 用于在模块更新后触发的函数
)
```

### `decline`

拒绝给定`依赖模块`的更新，使用`'decline'`方法强制更新失败。

```js
module.hot.decline(
  dependencies // 可以是一个字符串或字符串数组
)
```

### `dispose`（或`addDisposeHandler`）

添加一个处理函数，在当前模块代码被替换时执行。此函数应该用于移除你声明或创建的任何持久资源。如果要将状态传入到更新过的模块，请添加给定`data`参数。更新后，此对象在更新之后可通过`module.hot.data`调用。

```js
module.hot.dispose(data => {
  // 清理并将 data 传递到更新后的模块……
})
```

### `removeDisposeHandler`

删除由`dispose`或`addDisposeHandler`添加的回调函数。

```js
module.hot.removeDisposeHandler(callback)
```

### `status`

取得模块热替换进程的当前状态。

```js
module.hot.status() // 返回以下字符串之一……
```

### `check`

测试所有加载的模块以进行更新，如果有更新，则应用它们。

```js
module.hot.check(autoApply).then(outdatedModules => {
  // 超时的模块……
}).catch(error => {
  // 捕获错误
});
```

`autoApply`参数可以是布尔值，也可以是`options`，当被调用时可以传递给`apply`方法。

### `apply`

继续更新进程（只要`module.hot.status() === 'ready'`）。

```js
module.hot.apply(options).then(outdatedModules => {
  // 超时的模块……
}).catch(error => {
  // 捕获错误
});

```

可选的`options`对象可以包含以下属性：

| 属性 | 类型 | 描述 |
| :--- | :--- | :--- |
| `ignoreUnaccepted` | boolean | 忽略对未接受模块的更改。 |
| \`ignoreDeclined\` | boolean | 忽略对模块的更改。 |
| ignoreErrored | boolean | 忽略错误，在接收处理程序、错误处理程序和在重新执行模块时抛出错误。 |
| onDeclined | function\(info\) | declined模块提示器 |
| onUnaccepted | function\(info\) | Unaccepted模块提示器 |
| onAccepted | function\(info\) | accepted模块提示器 |
| onDisposed | function\(info\) | disposed模块提示器 |
| onErrored | function\(info\) | 错误提示器 |

函数中的`info`参数是包含下列一部分属性的对象：

```js
{
  type: "self-declined" | "declined" |
        "unaccepted" | "accepted" |
        "disposed" | "accept-errored" |
        "self-accept-errored" | "self-accept-error-handler-errored",
  moduleId: 4, // 模块的问题。
  dependencyId: 3, // 对于error: 模块id拥有accept处理程序。
  chain: [1, 2, 3, 4], // 对于 declined/accepted/unaccepted: 从何处传播更新的链。
  parentId: 5, // 对于 declined: declining 父类的模块id
  outdatedModules: [1, 2, 3, 4], // 对于 accepted: 已经过时的模块将会被处理掉
  outdatedDependencies: { // 对于 accepted: 接收将处理更新的处理程序的位置
    5: [4]
  },
  error: new Error(...), // 对于 errors: Pro出错误 error
  originalError: new Error(...) // 对于 self-accept-error-handler-errored:
                                //在错误处理程序试图处理它之前，模块抛出错误。
}
```

### `addStatusHandler`

注册一个函数来监听`status`的变化。

```js
module.hot.addStatusHandler(status => {
  // 响应当前状态……
})
```

### `removeStatusHandler`

移除一个注册的状态处理函数。

```js
module.hot.removeStatusHandler(callback)
```



