# **简介**

在学习之前，让我们先来探讨一下关于 webpack。

## webpack 是什么？

webpack是当下非常流行的一个Javascript应用程序模块打包器。还有一些其他的模块打包器，例如，[Browserify](http://browserify.org/) 或 [Brunch](http://brunch.io/)。而我们可能见过的如[grunt](https://gruntjs.com/)、[gulp](https://gulpjs.com/)等并不是模块打包起，他们不过是任务执行器。所谓任务执行器，就是用来自动化处理常见的开发任务，例如项目的检查\(lint\)、构建\(build\)、测试\(test\)。

这里又引申出两个新的概念：模块、和打包。

### 模块？

在[模块化编程](https://en.wikipedia.org/wiki/Modular_programming)中，开发者将程序分解成离散功能块\(discrete chunks of functionality\)，并称之为_模块。_每个模块具有比完整程序更小的接触面，使得校验、调试、测试轻而易举。 精心编写的_模块_提供了可靠的抽象和封装界限，使得应用程序中每个模块都具有条理清楚的设计和明确的目的。到处都能见到

### 打包？

## 为什么要使用 模块打包器（webpack）?

1. 前端工程化

## webpack 可以做什么？

webpack 可以将许多模块打包成几个捆绑在一起的资源（assets）。它的代码分割（Code Splitting）功能允许按需加载应用程序的一部分。通过webpack的很多`loader`，这些被打包的模块可以是CommonJs、AMD、ES6 模块、CSS、Image、JSON、Coffeescrit、LESS...等等，甚至是你自定义的，可以说在webpack中任何东西模块。这些模块都可以被webpack打包。

官方图：

![](/assets/what-is-webpack.png)

## webpack是如何工作的？

# 结构

先看看整体结构：

```js
module.exports = {
    entry: '',
    output: '',
    module: '',
    resolve: '',
    performance: '',
    devtool: '',
    context: '',
    target: '',
    externals: '',
    stats: '',
    devServer: '',
    plugins: '',

    //更多高级配置
    resolveLoader: '',
    parallelism: '',
    profile: '',
    bail: '',
    cache: '',
    watch: '',
    watchOptions: '',
    node: '',
    recordsPath: '',
    recordsInputPath: '',
    recordsOutputPath: ''

}
```



