# DefinePlugin

`DefinePlugin`允许创建一个在**编译**时可配置的全局常量。这可能会对开发模式和发布模式的构建允许不同的行为非常有用。如果在开发构建中，而不在发布构建中执行日志记录，则可以使用全局常量来决定是否记录日志。这就是`DefinePlugin`的用处，设置它，就可以忘记开发和发布构建的规则。

```
new webpack.DefinePlugin({
  // Definitions...
})
```

## 用法

每个传进`DefinePlugin`的键值都是一个标志符或者多个用`.`连接起来的标志符。

* 如果这个值是一个字符串，它会被当作一个代码片段来使用。
* 如果这个值不是字符串，它会被转化为字符串\(包括函数\)。
* 如果这个值是一个对象，它所有的 key 会被同样的方式定义。
* 如果在一个 key 前面加了`typeof`,它会被定义为 typeof 调用。

这些值会被内联进那些允许传一个代码压缩参数的代码中，从而减少冗余的条件判断。

```js
new webpack.DefinePlugin({
  PRODUCTION: JSON.stringify(true),
  VERSION: JSON.stringify("5fa3b9"),
  BROWSER_SUPPORTS_HTML5: true,
  TWO: "1+1",
  "typeof window": JSON.stringify("object")
})
```

```js
console.log("Running App version " + VERSION);
if(!BROWSER_SUPPORTS_HTML5) require("html5shiv");
```

> **\[info\]** 注：
>
> 因为这个插件直接执行文本替换，给定的值**必须包含**字符串本身内的**实际引号**。通常，有两种方式来达到这个效果，使用`'"production"'`, 或者使用`JSON.stringify('production')`。

**index.js**

```js
if (!PRODUCTION) {
  console.log('Debug info')
}

if (PRODUCTION) {
  console.log('Production log')
}
```

通过没有使用压缩的 webpack 的结果：

```js
if (!true) {
  console.log('Debug info')
}
if (true) {
  console.log('Production log')
}
```

通过使用压缩的 webpack 的结果:

```js
console.log('Production log')
```

## 功能标记\(Feature Flags\)

使用 [功能标记](https://en.wikipedia.org/wiki/Feature_toggle) 来「启用/禁用」「生产/开发」构建中的功能。

```js
new webpack.DefinePlugin({
  'NICE_FEATURE': JSON.stringify(true),
  'EXPERIMENTAL_FEATURE': JSON.stringify(false)
})
```

> **\[warning\]**  注：
>
> 当定义流程的值时，相比之下，`'process.env.NODE_ENV': JSON.stringify('production')`这种定义要好于`process: { env: { NODE_ENV: JSON.stringify('production') } }`。使用后者将覆盖`process`对象，这样可能破坏与某些模块的兼容性，这些模块期望在流程对象上定义其他值。

## 服务 URL\(Service URLs\)

在「生产/开发」构建中使用不同的服务 URL\(Service URLs\)：

```js
new webpack.DefinePlugin({
  'SERVICE_URL': JSON.stringify("http://dev.example.com")
})
```



