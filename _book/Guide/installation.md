# 安装方法

**安装最新版本**:

```bash
npm install --save-dev webpack
```

**安装指定版本**：

```bash
npm install --save-dev webpack@<version>
```

**最新体验版**：

```bash
npm install webpack@beta
npm install webpack/webpack#<tagname/branchname>
```

注：体验版中可能含有bug。

**全局安装**：

```bash
npm install --global webpack
```

**注：**对于大多数的项目我们都推荐在项目中安装webpack，这可以使我们在引入破坏式变更\(breaking change\)的依赖时，更容易分别升级项目。通常，webpack 通过运行一个或多个 [npm scripts](https://docs.npmjs.com/misc/scripts)，会在本地`node_modules`目录中查找安装的 webpack：

```js
"scripts": {
    "start": "webpack --config webpack.config.js"
}
```



