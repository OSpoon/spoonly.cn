# 介绍VSCode扩展结构

在默认创建的扩展中主要做了三件事情：

1. 注册 `onCommand` 激活事件，1.74 版本前要在 `activationEvents` 中显示列出；
2. 在清单文件中通过 `contributes` 提供 `Hello World` 命令；
3. 使用 `commands.registerCommand` 将命令 ID 和执行函数绑定。

## 扩展文件结构

```
vscode-extension-sample
.vscode             
├─ launch.json      				VSCode调试配置 
├─ tasks.json               VSCode任务配置   
├─ src                          
│  └─ extension.ts          扩展程序源码文件                           
├─ package.json             扩展程序清单文件           
├─ tsconfig.json            Typescript 配置    
└─ webpack.config.js        Webpack 编译配置  
```

## 扩展清单文件

`package.json` 文件在 VSCode 扩展程序中还充当着清单文件的作用：

```json
{
  "name": "hello-world",            // 扩展程序的唯一标识由`<publisher>.<name>`组成
  "publisher": "vscode-samples",
  "main": "./dist/extension.js",    // 扩展程序的入口文件
  "activationEvents": [],           // 扩展激活事件
  "contributes": {                  // 扩展贡献点（配置命令集）
    "commands": [
      {
        "command": "helloworld.helloWorld",
        "title": "Hello World"
      }
    ]
  },
  "engines": {					  // 扩展程序依赖 VSCodeAPI 的最低版本
    "vscode": "^1.92.0"
  },
  "scripts": {},          // 扩展使用的命令
  "devDependencies": {}   // 扩展依赖的模块
}

```

## 扩展入口文件

入口文件提供两个函数的导出，当注册事件激活时执行 activate 函数，当扩展停用前执行 deactivate 函数；VSCode 扩展程序的 API 由 `@types/vscode` 提供类型定义。

```typescript
import * as vscode from 'vscode';

export function activate(context: vscode.ExtensionContext) {
  
  console.log('Congratulations, your extension "helloworld-sample" is now active!');

  let disposable = vscode.commands.registerCommand('helloworld.helloWorld', () => {
    vscode.window.showInformationMessage('Hello World!');
  });

  context.subscriptions.push(disposable);
}

export function deactivate() {}
```

