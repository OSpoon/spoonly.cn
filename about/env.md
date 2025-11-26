---
title: "开发环境搭建 (MacOS)"
---

## 安装篇

### 必备

| 软件                                                            | 作用                        |
| --------------------------------------------------------------- | --------------------------- |
| [Chrome](https://www.google.cn/intl/zh-CN/chrome/)              | 同步 Google 账号            |
| [Ghelper](https://ghelper.net)                                  | 辅助上网（插件、客户端）    |
| [VS Code](https://code.visualstudio.com/)                       | 同步 VSC 配置、代码编辑器   |
| [Github Desktop](https://github.com/apps/desktop)               | 管理代码仓库、附带 Git 环境 |
| [Command Line Tools](https://developer.apple.com/download/all/) | 安装 NVM 时需要依赖         |

### 辅助工具

| 软件                                             | 作用           |
| ------------------------------------------------ | -------------- |
| [Rectangle](https://rectangleapp.com/)           | 屏幕窗口管理器 |
| [SnipPaste](https://www.snipaste.com/)           | 截图工具       |
| [iFan](https://www.better365.cn/h-col-195.html)  | Mac 风扇管理器 |
| [FastZip](https://www.better365.cn/fastzip.html) | 解压工具       |

### 文档编辑

| 软件                                  | 作用            |
| ------------------------------------- | --------------- |
| [Typora](https://typora.io/)          | Markdown 编辑器 |
| [uPic](https://github.com/gee1k/uPic) | 图片上传工具    |

### 安装 [Node.js](https://nodejs.org/)

> 需要配置系统代理、安装 **Command Line Tools**

```bash
# 安装 nvm ( Node 版本管理器)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash

# 下载并安装 Node.js (建议重启终端后安装)
nvm install 20

# 验证当前环境中 Node.js 和 npm 的版本
node -v # should print `v20.16.0`
npm -v # should print `10.8.1`
```

### 安装 [Oh My Zsh](https://ohmyz.sh)

> 需要配置系统代理、安装 **Git** 环境

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

PS：安装后需要重新配置 **重启终端生效** 部分。

## 配置篇

### 配置系统代理

```bash
# 配置文件位置：~/.bash_profile
export https_proxy=http://127.0.0.1:7890
export http_proxy=http://127.0.0.1:7890
export all_proxy=socks5://127.0.0.1:7890
```

### 重启终端生效

```bash
# 配置文件位置：~/.zshrc
source ~/.bash_profile
```

### 配置 `code` 命令到 PATH

1. 启动 VS Code 命令窗口：`command + shift + p`
2. 输入 `shell command`
3. 选择 Install `code` command in PATH

### NPM 全局模块

- @antfu/ni：Adaptive packet manager
- nrm：Node.js NPM registry manager
- cross-env：Set environment variables
- command-line-toolbox：Command line toolbox
- degit：Copy git repository
- pnpm：Fast, disk space efficient package manager
- yarn：Fast, reliable, and secure dependency management.

```bash
npm i -g @antfu/ni nrm cross-env command-line-toolbox degit pnpm yarn
```

### NPM 代理管理:

设置代理

```bash
npm config set proxy http://127.0.0.1:7890
```

```bash
npm config set https-proxy http://127.0.0.1:7890
```

查看代理

```bash
npm config get proxy
```

```bash
npm config get https-proxy
```

删除代理

```bash
npm config delete proxy
```

```bash
npm config delete https-proxy
```

### Git 代理管理:

设置代理

```bash
git config --global http.proxy http://127.0.0.1:7890
```

```bash
git config --global https.proxy http://127.0.0.1:7890
```

查看代理

```bash
git config --global --get http.proxy
```

```bash
git config --global --get https.proxy
```

删除代理

```bash
git config --global --unset http.proxy
```

```bash
git config --global --unset https.proxy
```

## 快捷键

| 级别    | 作用          | 快捷键                |
| ------- | ------------- | --------------------- |
| 系统    | 显示/隐藏文件 | `command + shift + .` |
| VS Code | 打开命令窗口  | `command + shift + p` |
