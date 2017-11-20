# Detools（开发工具）

该选项控制是否生成源映射（source maps）以及如何生成源映射。

使用[`SourceMapDevToolPlugin`](/plugins/source-map-dev-tool-plugin)来进行更细粒度的配置。请参阅[`source-map-loader`](/loaders/source-map-loader)来处理现有的源映射。

## `devtool`

类型：`string` \| `false`

选择一种类型的[source mapping](http://blog.teamtreehouse.com/introduction-source-maps) 来增强调试过程。这些值会显著地影响构建和重建速度。

> **\[info\]** 注：
>
> webpack仓库包含一个[示例显示了所有`devtool`变量的效果](#)。这些例子可能会帮助你理解这些差异。

-

> **\[info\]** 注：
>
> 替代使用devtool选项，你还可以使用`SourceMapDevToolPlugin` / `EvalSourceMapDevToolPlugin`直接拥有更多的选择。永远不要同时使用`devtool`选项和插件。`devtool`选项在内部添加了插件，这样你就会使用两次插件。

| devtool | build（构建） | rebuild（重新构建） | production（生产） | quality（品质） |
| --- | --- | --- | --- | --- |
| \(none\) | +++ | +++ | no | bundled code（打包代码） |
| eval | +++ | +++ | no | generated code（生成代码） |
| cheap-eval-source-map | + | ++ | no | transformed code \(lines only\)转换代码 |
| cheap-module-eval-source-map | o | ++ | no | original source \(lines only\)原始源 |
| eval-source-map | -- | + | no | original source（原始源） |
| cheap-source-map | + | o | no | transformed code \(lines only\)转换代码 |
| cheap-module-source-map | o | - | no | original source \(lines only\)原始元 |
| inline-cheap-source-map | + | o | no | transformed code \(lines only\)转换代码 |
| inline-cheap-module-source-map | o | - | no | original source \(lines only\)原始源 |
| source-map | -- | -- | yes | original source |
| inline-source-map | -- | -- | no | original source |
| hidden-source-map | -- | -- | yes | original source |
| nosources-source-map | -- | -- | yes | without source content（不带源内容） |

> **\[info\]** 注：
>
> `+++` 特别快, `++` 非常快, `+`  一般快, `o` 中等, `-` 非常慢 , `--` 特别慢

其中一些值适合于开发，一些用于生产。对于开发，您通常需要以包大小为代价的快速源映射（Source Maps ），但是对于生产，您需要独立的源映射（Source Maps），这些映射是准确的和支持最小化的。

> **\[warning\]** 注：
>
> 在Chrome中有一些Source Maps的问题。[我们需要你的帮助 !](https://github.com/webpack/webpack/issues/3165)

-

> **\[info\]** 注：
>
> 查看 [`output.sourceMapFilenam`](/configuration/output#output-sourcemapfilename) 来自定义生成的Source Maps的文件名。

### Qualities （品质）

`bundled code` - You see all generated code as a big blob of code. You don't see modules separated from each other.您将所有生成的代码视为一大块代码。你看不到模块之间的相互分离。

`generated code` - 您可以看到每个模块彼此分离，并带有模块名的注释。您可以看到由webpack生成的代码。例如： 你可以看到像 `var module__WEBPACK_IMPORTED_MODULE_1__ = __webpack_require__(42); module__WEBPACK_IMPORTED_MODULE_1__.a();`的一些东西，而不是`import {test} from "module"; test();`

`transformed code` - 您可以看到每个模块彼此分离，并带有模块名的注释。您可以在webpack转换代码之前看到这些代码，但是在Loader反编译这些代码之后。例如： 你可以看到像  `import {test} from "module"; var A = function(_test) { ... }(test);`的一些东西，而不是 `import {test} from "module"; class A extends test {}`

`original source` - 您可以看到每个模块彼此分离，并带有模块名的注释。当你在编写代码的时候，你可以在反编译之前看到这些代码，在您编写它之前，您可以看到代码之前的代码。这取决于Loader的支持。

`without source content` - 源中的内容不包含在源映射（Source Maps）中。浏览器通常尝试从web服务器或文件系统加载源。你必须确保正确地设置[`devtoolModuleFilenameTemplate`](/configuration/output#output.devtoolModuleFilenameTemplate)来匹配源url。

`(lines only)` - 源映射（Source Maps）被简化为每行一个映射。这通常意味着每个语句都有一个映射\(假设您是这样做的\)。这可以防止您在语句级别上调试执行，以及在一行的列上设置断点。结合最小化是不可能的，因为最小值通常只会发出（emit）一条单一直线。

### 开发

下面的选项是开发的理想选择:

`eval` - 每个模块都使用`eval()`和`//@ sourceURL`执行。这是非常快。主要的缺点是它没有正确地显示行号，因为它被映射到被转换的代码而不是原始代码\(没有来自Loader的源映射\)。

`eval-source-map` - 最初它是缓慢的，但是它提供了快速的重建速度并产生真实的文件。由于映射到原始代码，所以行号被正确映射。它为开发提供了最好的质量源映射。

`cheap-eval-source-map` - 与`eval-source-map`类似，每个模块都用`eval()`执行。它是“便宜”的，因为它没有列映射，它只映射行号。它忽略了来自Loader的源程序，只显示了与`eval` 开发工具类似的反编译代码。

`cheap-module-eval-source-map` - 与`cheap-eval-source-map`类似，但是，在这种情况下，它会处理来自Loader的源映射以得到更好的借过。然而，Loader源映射\(Loader Source Maps\)被简化为每行一个映射。

### 特殊情况

下面的选项不适合开发和生产。对于一些特殊的情况，需要一些特殊的选项，比如一些第三方工具。

`inline-source-map` - 一个SourceMap作为一个DataUrl被添加到bundle中。

`cheap-source-map` -一个没有列映射的SourceMap，忽略了loader源映射

`inline-cheap-source-map` -类似于`cheap-source-map` ，但是SourceMap作为一个DataUrl被添加到bundle中。

`cheap-module-source-map` - 一个没有列映射的SourceMap，它简化了loader源映射为每行一个映射。

`inline-cheap-module-source-map` -类似于 `cheap-module-source-map` ，但是SourceMap作为一个DataUrl被添加到bundle中。

### 生产

These options are typically used in production:

`(none)` \(Omit the `devtool` option\) - No SourceMap is emitted. This is a good option to start with.

`source-map` - A full SourceMap is emitted as a separate file. It adds a reference comment to the bundle so development tools know where to find it.

W&gt; You should configure your server to disallow access to the Source Map file for normal users!

`hidden-source-map` - Same as `source-map`, but doesn't add a reference comment to the bundle. Useful if you only want SourceMaps to map error stack traces from error reports, but don't want to expose your SourceMap for the browser development tools.

W&gt; You should not deploy the Source Map file to the webserver. Instead only use it for error report tooling.

`nosources-source-map` - A SourceMap is created without the `sourcesContent` in it. It can be used to map stack traces on the client without exposing all of the source code. You can deploy the Source Map file to the webserver.

W&gt; It still exposes filenames and structure for decompiling, but it doesn't expose the original code.

T&gt; When using the `uglifyjs-webpack-plugin` you must provide the `sourceMap: true` option to enable SourceMap support.

