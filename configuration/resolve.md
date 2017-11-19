# Resolve（解析）

这些选项能设置模块如何被解析。webpack 提供合理的默认值，但是还是可能会修改一些解析的细节。关于 resolver 具体如何工作的更多解释说明，请查看[模块解析方式](https://doc.webpack-china.org/concepts/module-resolution)。

```js
module.exports = {
    resolve: {
        alias: '',
        aliasFields: '',
        cacheWithContext: '',
        descriptionFiles: '',
        enforceExtension: '',
        enforceModuleExtension: '',
        extensions: '',
        mainFields: '',
        mainFiles: '',
        modules: '',
        unsafeCache: '',
        plugins: '',
        symlinks: '',
        cachePredicate: '',
        resolveLoader: {
            moduleExtensi
        }
    }
}
```

## `resolve`

类型：object

配置模块如何解析。例如，当在 ES2015 中调用`import "lodash"`，`resolve`选项能够对 webpack 查找`"lodash"`的方式去做修改（查看[`模块`](https://doc.webpack-china.org/configuration/resolve/#resolve-modules)）。

### `resolve.alias`

类型：object

创建`import`或`require`的别名，来确保模块引入变得更简单。例如，一些位于`src/`文件夹下的常用模块：

```js
alias: {
  Utilities: path.resolve(__dirname, 'src/utilities/'),
  Templates: path.resolve(__dirname, 'src/templates/')
}
```

使用相对路径：

```js
import Utility from '../../utilities/utility';
```

采用别名：

```js
import Utility from 'Utilities/utility';
```

也可以在给定对象的键后的末尾添加`$`，以表示精准匹配：

```js
alias: {
  xyz$: path.resolve(__dirname, 'path/to/file.js')
}
```

这将产生以下结果：

```js
import Test1 from 'xyz'; // 精确匹配，所以 path/to/file.js 被解析和导入
import Test2 from 'xyz/file.js'; // 精确匹配，触发普通解析
```

下面的表格展示了一些其他情况：

| `别名：` | `import "xyz"` | `import "xyz/file.js"` |
| :--- | :--- | :--- |
| `{}` | `/abc/node_modules/xyz/index.js` | `/abc/node_modules/xyz/file.js` |
| `{ xyz: "/abs/path/to/file.js" }` | `/abs/path/to/file.js` | error |
| `{ xyz$: "/abs/path/to/file.js" }` | `/abs/path/to/file.js` | `/abc/node_modules/xyz/file.js` |
| `{ xyz: "./dir/file.js" }` | `/abc/dir/file.js` | error |
| `{ xyz$: "./dir/file.js" }` | `/abc/dir/file.js` | `/abc/node_modules/xyz/file.js` |
| `{ xyz: "/some/dir" }` | `/some/dir/index.js` | `/some/dir/file.js` |
| `{ xyz$: "/some/dir" }` | `/some/dir/index.js` | `/abc/node_modules/xyz/file.js` |
| `{ xyz: "./dir" }` | `/abc/dir/index.js` | `/abc/dir/file.js` |
| `{ xyz: "modu" }` | `/abc/node_modules/modu/index.js` | `/abc/node_modules/modu/file.js` |
| `{ xyz$: "modu" }` | `/abc/node_modules/modu/index.js` | `/abc/node_modules/xyz/file.js` |
| `{ xyz: "modu/some/file.js" }` | `/abc/node_modules/modu/some/file.js` | error |
| `{ xyz: "modu/dir" }` | `/abc/node_modules/modu/dir/index.js` | `/abc/node_modules/dir/file.js` |
| `{ xyz: "xyz/dir" }` | `/abc/node_modules/xyz/dir/index.js` | `/abc/node_modules/xyz/dir/file.js` |
| `{ xyz$: "xyz/dir" }` | `/abc/node_modules/xyz/dir/index.js` | `/abc/node_modules/xyz/file.js` |

如果在`package.json`中定义，`index.js`可能会被解析为另一个文件。

`/abc/node_modules`也可能在`/node_modules`中解析。

### `resolve.aliasFields`

类型：string

指定一个字段，例如`browser`，根据[此规范](https://github.com/defunctzombie/package-browser-field-spec)进行解析。默认：

```js
aliasFields: ["browser"]
```

### `resolve.cacheWithContext`

类型：boolean

如果启用了不安全的缓存，则在缓存的键中包括`request.context`。这个选项是由[`enhanced-resolve`](https://github.com/webpack/enhanced-resolve/)模块考虑的。从webpack 3.1.0开始，在解析或提供了resolveLoader插件时，解析缓存中的上下文会被忽略。这解决了性能倒退的问题。

### `resolve.descriptionFiles`

类型：array

用于描述的 JSON 文件。默认：

```js
descriptionFiles: ["package.json"]
```

### `resolve.enforceExtension`

类型：boolean

如果是`true`，将不允许无扩展名\(extension-less\)文件。默认如果`./foo`有`.js`扩展，`require('./foo')`可以正常运行。但如果启用此选项，只有`require('./foo.js')`能够正常工作。默认：

```js
enforceExtension: false
```

### `resolve.enforceModuleExtension`

类型：boolean

对模块是否需要使用的扩展（例如 loader）。默认：

```js
enforceModuleExtension: false
```

### `resolve.extensions`

类型：array

自动解析确定的扩展。默认值为：

```js
extensions: [".js", ".json"]
```

能够使用户在引入模块时不带扩展：

```js
import File from '../path/to/file'
```

> ** \[warning\]注：**
>
> 使用此选项，会**覆盖默认数组**，这就意味着 webpack 将不再尝试使用默认扩展来解析模块。对于使用其扩展导入的模块，例如，`import SomeFile from "./somefile.ext"`，要想正确的解析，一个包含“\*”的字符串必须包含在数组中。

### `resolve.mainFields`

### `resolve.mainFiles`

### `resolve.modules`

### `resolve.unsafeCache`

### `resolve.plugins`

### `resolve.symlinks`

### `resolve.cachePredicate`

### `resolveLoader`

### `resolveLoader.moduleExtensions`

### 



