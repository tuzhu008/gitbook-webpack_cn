# 开始

## 新建项目

选择适当的目录，然后在终端运行：

```bash
# 创建文件夹，并进入文件夹
mkdir my-webpack && cd my-webpack

# 初始化项目，这时候会在my-webpack文件夹下创建一个package.json文件
npm init -y

# 安装webpack
npm install --save-dev webpack
```

执行完毕后，当前目录结构如下：

```js
my-webpack
├── node_modules // 依赖模块
├── package-lock.json // 记录当前node_modules文件夹下的依赖项
└── package.json //配置文件
```



