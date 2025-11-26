# 开发VSCode扩展第一步



## 配置开发环境

1. 首先全局安装 [Yeoman](https://www.npmjs.com/package/yo) 和 [generator-code](https://www.npmjs.com/package/generator-code)

```shell
npm i --g yo generator-code
```

2. 接着运行启动脚手架开始创建项目：

```shell
 yo code
```

3. 第一次创建建议选择 **New Extension (TypeScript)** 模板作为基础：

```shell
? What type of extension do you want to create? New Extension (TypeScript)
? What's the name of your extension? HelloWorld
? What's the identifier of your extension? hello-world
? What's the description of your extension? 
? Initialize a git repository? Yes
? Which bundler to use? webpack
? Which package manager to use? npm
```

## 运行扩展程序

1. 安装成功后使用 VSCode 打开项目，按 F5 开启调试；
2. 在新打开的窗口通过命令面板运行“Hello World”命令，在 VSCode 右下角将看到提示 “Hello World from helloworld!”。

## 开发扩展程序

1. 紧接着修改 “'Hello World from helloworld!'” 为任意内容；
2. 在新打开的窗口通过命令面板运行“Developer: Reload Window” 命令，重新加载窗口；
3. 再次运行“Hello World”命令，VSCode 右下角的提示将更新为最新内容。

## 调试扩展程序

扩展程序运行后即进入了调试模式，为源码指定的行添加断点，程序运行到指定行时将触发断点。
