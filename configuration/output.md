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

**使用入口名称**：

```js
filename: "[name].bundle.js"
```

**使用内部chunk id：**

```js
filename: "[id].bundle.js"
```

**使用为每一次构建（build）生成的hash：**

```js
filename: "[name].[hash].bundle.js"
```

**使用为每一个chunk生成的hash：**

```js
filename: "[chunkhash].bundle.js"
```

> ** \[info\] 注 **
>
> 当文件不发生改变时，chunkhash 不发生变化。由此可以利用来进行缓存，加快加载速度。
>
> chunkhash 在使用开发服务时无法使用。
>
> 此选项不会影响那些按需加载的chunk，控制这些文件的输出，请使用[output.chunFilename](#outputchunkfilename)。

可用模板字符串：

| 模板 | 描述 |
| :--- | :--- |
| \[hash\] | 构建相关的hash |
| \[chunkhash\] | chunk内容相关的hash |
| \[name\] | 模块名称 |
| \[id\] | chun id |
| \[query\] | 模块的查询字符串，\`?\`后的部分 |

`[hash]`和`[chunkhash]`的长度可以使用`[hash:16]`（默认为20）来指定。

## output.chunkFilename

类型：string

此选项决定了非入口\(non-entry\) chunk 文件的名称。可取值与[output.filename](#outputfilename) 相同。

注意，这些文件名需要在 runtime 根据 chunk 发送的请求去生成。因此，需要在 webpack runtime 输出 bundle 值时，将 chunk id 的值对应映射到占位符(如 [name] 和 [chunkhash])。这会增加文件大小，并且在任何 chunk 的占位符值修改后，都会使 bundle 失效。

默认使用 [id].js 或从 output.filename 中推断出的值（[name] 会被预先替换为 [id] 或 [id].）。


