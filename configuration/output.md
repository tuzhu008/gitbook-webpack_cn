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

注意，这些文件名需要在 runtime 根据 chunk 发送的请求去生成。因此，需要在 webpack runtime 输出 bundle 值时，将 chunk id 的值对应映射到占位符\(如 \[name\] 和 \[chunkhash\]\)。这会增加文件大小，并且在任何 chunk 的占位符值修改后，都会使 bundle 失效。

默认使用 `[id].js` 或从 output.filename 中推断出的值（\[name\] 会被预先替换为 \[id\] 或 \[id\].）。

## output.publicPath

类型：string

默认值：“”，空字符串

此配置选项用于指定在浏览器中所引用的此输出目录对应的URL。相对URL会被相对于HTML页面进行解析。相对于服务器的URL、相对于协议的URL、绝对URL也可能会用到。当资源托管到CDN时，必须要设置此选项。

该选项的值是以 runtime\(运行时\) 或 loader\(载入时\) 所创建的每个 URL 为前缀。因此，在多数情况下，此选项的值都会以/结束。

以HTML页面为基准：

```js
path: path.resolve(__dirname, "public/assets");
publicPath: "https://cdn.example.com/assets/";
```

项目所有的地址，都会加上publicPath的前缀。

```js
publicPath: "/assets/";
chunkFilename: "[id].chunk.js";
```

这个chunk请求的地址大概为`/assets/[id].chunk.js`

```js
<link href="/assets/spinner.gif" />
```

```js
background-image: url(/assets/spinner.gif);
```

webpack-dev-server也会以publicPath为基础，以决定在哪个目录下启动服务，来访问输出的文件。

示例：

```js
publicPath: "https://cdn.example.com/assets/", // CDN（总是 HTTPS 协议）
publicPath: "//cdn.example.com/assets/", // CDN (协议相同)
publicPath: "/assets/", // 相对于服务(server-relative)
publicPath: "assets/", // 相对于 HTML 页面
publicPath: "../assets/", // 相对于 HTML 页面
publicPath: "", // 相对于 HTML 页面（目录相同）
```

在编译时\(compile time\)无法知道输出文件的 publicPath 的情况下，可以留空，然后在入口文件\(entry file\)处使用自由变量\(free variable\) webpack\_public\_path，以便在运行时\(runtime\)进行动态设置。

```js
 __webpack_public_path__ = myRuntimePublicPath

// 应用程序入口的其他部分
```

# output.sourceMapFillename

类型：string

此选项会向硬盘写入一个输出文件，只在 `devtool` 启用了 SourceMap 选项时才使用。

配置 source map 的命名方式。默认使用`"[file].map"`。

可以使用 [\#output-filename ](#outputfilename)中的 `[name]`, `[id]`, `[hash]` 和 `[chunkhash]` 替换符号。除此之外，还可以使用以下替换符号。`[file]` 占位符会被替换为原始文件的文件名。我们建议只使用 `[file]` 占位符，因为其他占位符在非 chunk 文件生成的 SourceMap 时不起作用。

| 模板 | 描述 |
| :--- | :--- |
|  |  |



