# NpmInstallWebpackPlugin

通过**自动安装和保存** Webpack的依赖关系来加快开发。

您的构建脚本和服务器（script & server）只是安装一个您以前不知道您需要的依赖项，这很糟糕。

相反，你只需像平常一样使用`require`或`import`，`npm install`会在你工作的时候**自动地安装和保存**丢失的依赖。

## 安装

```js
npm install --save-dev npm-install-webpack-plugin
```

## 用法

在`webpack.config.js`中：

```js
plugins: [
  new NpmInstallPlugin()
]
```

**相当于**：

```js
plugins: [
  new NpmInstallPlugin({
    // 使用 --save 或者 --save-dev
    dev: false,
    // 安装缺少的 peerDependencies
    peerDependencies: true,
    // 减少控制台日志记录的数量
    quiet: false,
    // 在公司内部使用的npm命令，不支持 yarn
    npm: 'tnpm'
  });
],
```

可以提供一个`Function`来动态设置`dev`：

```js
plugins: [
  new NpmInstallPlugin({
    dev: function(module, path) {
      return [
        "babel-preset-react-hmre",
        "webpack-dev-middleware",
        "webpack-hot-middleware",
      ].indexOf(module) !== -1;
    },
  }),
],
```

## demo

![](/assets/plugins-npm.gif)

## 特性

* \[x\] Works with both Webpack ^v1.12.0 and ^2.1.0-beta.0.
* \[x\] Auto-installs .babelrc plugins & presets.
* \[x\] Supports both ES5 & ES6 Modules. \(e.g. require, import\)
* \[x\] Supports Namespaced packages. \(e.g. @cycle/dom\)
* \[x\] Supports Dot-delimited packages. \(e.g. lodash.capitalize\)
* \[x\] Supports CSS imports. \(e.g. @import "~bootstrap"\)
* \[x\] Supports Webpack loaders. \(e.g. babel-loader, file-loader, etc.\)
* \[x\] Supports inline Webpack loaders. \(e.g. require\("bundle?lazy!./App"\)
* \[x\] Auto-installs missing peerDependencies. \(e.g. @cycle/core will automatically install rx@\*\)
* \[x\] Supports Webpack's resolve.alias & resolve.root configuration. \(e.g. require\("react"\) can alias to react-lite\)



