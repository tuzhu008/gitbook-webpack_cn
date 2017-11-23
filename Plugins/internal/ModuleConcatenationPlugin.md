# ModuleConcatenationPlugin

过去 webpack 打包时的一个取舍是将 bundle 中各个模块单独打包成闭包。这些打包函数使你的 JavaScript 在浏览器中处理的更慢。相比之下，一些工具像 Closure Compiler 和 RollupJS 可以提升\(hoist\)或者预编译所有模块到一个闭包中，提升你的代码在浏览器中的执行速度。

这个插件会在 webpack 中实现以上的预编译功能。

```js
new webpack.optimize.ModuleConcatenationPlugin()
```

> **\[info\]**
>
> 这种连结行为被称为“作用域提升\(scope hoisting\)”。
>
> 由于实现 ECMAScript 模块语法，作用域提升\(scope hoisting\)这个特定于此语法的功能才成为可能。`webpack`可能会根据你正在使用的模块类型和[其他的情况](https://medium.com/webpack/webpack-freelancing-log-book-week-5-7-4764be3266f5)，回退到普通打包。

## 绑定失败的优化（Optimization Bailouts）

正如文章所解释的，webpack试图实现局部作用域提升\(scope hoisting\)。它将把模块合并到一个单一的作用域中，但是在任何情况下都不能这样做。如果webpack不能合并一个模块，那么两个替代方案就是Prevent 和 Root。Prevent 意味着模块必须在自己的作用域内。Root意味着将创建一个新的模块组。以下条件决定了结果:

| 条件 | 结果 |
| :--- | :--- |
| Non ES6 Module | Prevent |
| Imported By Non Import | Root |
| Imported From Other Chunk | Root |
| Imported By Multiple Other Module Groups | Root |
| Imported With `import()` | Root |
| Affected By ProvidePlugin Or Using module | Prevent |
| HMR Accepted | Root |
| Using `eval()` | Prevent |
| In Multiple Chunks | Prevent |
| `export * from "cjs-module"` | Prevent |



## 模块分组算法

以下 JavaScript 伪代码解释了算法：

```js
modules.forEach(module => {
  const group = new ModuleGroup({
    root: module
  });
  module.dependencies.forEach(dependency => {
    tryToAdd(group, dependency);
  });
  if (group.modules.length > 1) {
    orderedModules = topologicalSort(group.modules);
    concatenatedModule = new ConcatenatedModule(orderedModules);
    chunk.add(concatenatedModule);
    orderedModules.forEach(groupModule => {
      chunk.remove(groupModule);
    });
  }
});

function tryToAdd(group, module) {
  if (group.has(module)) {
    return true;
  }
  if (!hasPreconditions(module)) {
    return false;
  }
  const nextGroup = group;
  const result = module.dependents.reduce((check, dependent) => {
    return check && tryToAdd(nextGroup, dependent);
  }, true);
  if (!result) {
    return false;
  }
  module.dependencies.forEach(dependency => {
    tryToAdd(group, dependency);
  });
  group.merge(nextGroup);
  return true;
}
```

### 优化绑定失败的调试\[Debugging Optimization Bailouts\]

当我们使用 webpack CLI 时，加上参数`--display-optimization-bailout`将显示绑定（bailout）失败的原因。在 webpack 配置里，只需将以下内容添加到 stats 对象中：

```js
{
  ...stats,
  // Examine all modules
  maxModules: Infinity,
  // Display bailout reasons
  optimizationBailout: true
}
```



