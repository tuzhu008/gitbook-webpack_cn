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



