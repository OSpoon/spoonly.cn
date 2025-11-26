# VSCode扩展指南-Command

命令用来在 VSCode 中触发操作，配置快捷键就是和命令在打交道，命令也可以被扩展程序使用，项用户展示功能，绑定到 UI 操作中并实现内部逻辑。

## 使用命令

VSCode 包含一个大型的[内置命令集](https://code.visualstudio.com/api/references/commands)，开发者可以使用这些命令与编辑器进行交互、控制用户界面或执行后台操作。许多扩展程序也会将其核心功能暴漏为命令，供用户和其他扩展程序利用。

### 编程式执行命令

通过内置的 `vscode.commands.executeCommand` API 可以实现以编程的方式执行命令。例如，执行下面的添加行注释，将所在行切换为对应语言环境的注释文本：

```typescript
import * as vscode from 'vscode';

function commentLine() {
  vscode.commands.executeCommand('editor.action.addCommentLine');
}
```

