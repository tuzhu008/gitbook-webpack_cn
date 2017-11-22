# EnvironmentPlugin

`EnvironmentPlugin`是一个通过[`DefinePlugin`](/Plugins/internal/DefinePlugin.md)来设置[`process.env`](https://nodejs.org/api/process.html#process_process_env)环境变量的快捷方式。

## 用法

```js
new webpack.EnvironmentPlugin(options)
```

## options

类型：object \| array

### array

`EnvironmentPlugin`可以接收一个键的数组。

```js
new webpack.EnvironmentPlugin(['NODE_ENV', 'DEBUG'])
```

上面的写法和下面这样使用`DefinePlugin`的效果相同：

```js
new webpack.DefinePlugin({
  'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV),
  'process.env.DEBUG': JSON.stringify(process.env.DEBUG)
})
```

> **\[info\]** 注：
>
> 使用不存在的环境变量会导致一个 "`EnvironmentPlugin`-`${key}`environment variable is undefined" 错误。

### Object

`EnvironmentPlugin`也可以接收一个指定相应默认值的对象，如果在`process.env`中对应的环境变量不存在时将使用指定的默认值。

```js
new webpack.EnvironmentPlugin({
  NODE_ENV: 'development', // 除非有定义 process.env.NODE_ENV，否则就使用 'development'
  DEBUG: false
})
```

> **\[warning\]** 注：
>
> 从`process.env`中取到的值类型均为字符串。

-

> **\[info\]** 注：
>
> 不同于[`DefinePlugin`](/Plugins/internal/DefinePlugin.md)，默认值将被`EnvironmentPlugin`执行`JSON.stringify`。

-

> **\[info\]** 注：
>
> 如果要指定一个未设定的默认值，使用`null`来代替`undefined`。

## 示例

让我们看一下对下面这个用来试验的文件 `entry.js` 执行前面配置的 `EnvironmentPlugin` 的结果：


```js
if (process.env.NODE_ENV === 'production') {
  console.log('Welcome to production');
}
if (process.env.DEBUG) {
  console.log('Debugging output');
}
```
当在终端执行 `NODE_ENV=production webpack` 来构建时，`entry.js` 变成了这样：



```js
if ('production' === 'production') { // <-- NODE_ENV 的 'production' 被带过来了
  console.log('Welcome to production');
}
if (false) { // <-- 使用了默认值
  console.log('Debugging output');
}
```
执行 `DEBUG=false webpack` 则会生成：



```js
if ('development' === 'production') { // <-- 使用了默认值
  console.log('Welcome to production');
}
if ('false') { // <-- DEBUG 的 'false' 被带过来了
  console.log('Debugging output');
}

```



