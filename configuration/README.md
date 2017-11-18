# 配置

整体配置结构

```js
module.exports = {
    entry: '', // 入口
    output: '', // 出口
    module: '', // 模块，配置处理项目中不同类型模块的方式
    resolve: '', // 解析，配置处理模块的解析方式
    performance: '', // 性能，控制 webpack 如何通知「资源(asset)和入口起点超过指定文件限制」
    devtool: '', // 通过在浏览器调试工具(browser devtools)中添加元信息(meta info)增强调试
    context: '', // 基础目录，是一个绝对路径，用于从配置中解析入口起点(entry point)和 loader
    target: '',  // 构建目标，webpack可以为多个环境构建编译
    externals: '', // 外部扩展，提供「从输出的 bundle 中排除依赖」的方法
    stats: '', // 统计， 准确地控制显示哪些包的信息
    devServer: '', // 开发服务
    plugins: '', // 插件，定制webpack构建过程

    // 更多高级配置
    loader: '', // 在 loader 上下文中暴露自定义值
    resolveLoader: '', // 独立解析选项的 loader
    parallelism: '', // 限制并行处理模块的数量
    profile: '', // 捕获时机信息
    bail: '', // 在第一个错误出现时抛出失败结果，而不是容忍它。
    cache: '', // 禁用/启用缓存
    watch: '', // 启用观察
    watchOptions: '', // 定制watch
    node: '', // 配置是否 polyfill 或 mock 某些 Node.js 全局变量和模块
    amd: '', // 设置 require.amd 或 define.amd 的值：
    recordsPath: '', // 打开这个选项可以生成包含 webpack 记录的 JSON 文件
    recordsInputPath: '', // 设定读取最后一条记录的文件的名称。
    recordsOutputPath: '' // 设定记录要写入的位置。

}
```



