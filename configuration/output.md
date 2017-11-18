# output（输出）

output用于配置一些关于webpack打包输出的信息。比如，输出的文件名、输出路径等等。

## output.path

类型：`string`

output 目录对应一个**绝对路径**。

```js
path: path.resolve(__dirname, 'dist/assets')
```

## output.filename

类型：`string`

这个选项决定了每个输出bundle的名称。这些bundle会被写入output.path选项指定的目录下。

```js
filename: 'main.bundle.js'
```

在多入口起点、代码分隔等会生成多个bundle的情况下，可以使用下面的占位符来命名：



