# 入口（entry）

entry用来配置webpack的入口文件（chunk），一个entry对应一个chunk。webpack从这里开始执行打包。

entry的取值类型：`string`、`array`、`object`、`function`

**string**:

```js
module.exports = {
    entry: "./src/index.js"
}
```

**array**:

```js
module.exports = {
    entry: [
        './src/index.js',
        './src/app.js'
    ]
}
```

entry使用字符串数组时，会将数组内的所有的依赖文件都打包到一个bundle中。

```
 **[info] **

>
>
 Use this for infomation messages.
```

**object**:

```js
module.exports = {
    entry: {
        app: './src/app.js',
        index: './src/index.js'
    }
}
```

entry使用对象时代表的是多入口文件，最后会打包生成和对象中的每一个入口文件相对应的bundle。entry对象的每一个键值（如上的app、index）在webpack中都会保留下来，可以用来配置输出文件的名字，使用字段`[name]`

**function**:

返回一个entry字符串、数组、或者对象的函数。

```js
module.exports = {
    entry: () => {
        return <string> || <array> || <object>
    }
}
```



