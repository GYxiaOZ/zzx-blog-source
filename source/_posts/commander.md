---
title: Commander.js
date: 2021-01-21 10:25:13
categories:
tags:
---

[Commander.js](https://github.com/tj/commander.js) 是用来搞 node.js 命令行的，现在通过一个简单的例子来学习如何使用 Commander.js

## 需求

做一个简单的用于 windows 创建文件的命令行，输入个数、前缀和后缀，创建文件或文件夹

## 想要实现的最终效果

## 命令

### `mk dir [path]`

### `mk file [path]`

`path` 文件或目录名

### 选项

`-n, --number <number>` 数量，如果传入则会创建形如 x0,x1,x2... 的文件或目录

## 初始化项目

需要用到的库：

- [commander](https://github.com/tj/commander.js)：创建命令行
- [chalk](https://github.com/chalk/chalk)：给命令行加颜色
- [inquirer](https://github.com/SBoudrias/Inquirer.js)：交互式命令行

既然是用于创建文件的，那我们就叫做 `mk-cli` 吧，在你的命令行工具依次执行以下命令：

```bash
mkdir mk-cli
cd mk-cli
yarn init -y
yarn add commander chalk inquirer
```

编辑器打开项目后，新建`index.js`文件作为我们的入口文件，文件第一行添加以下代码，意思是告诉工具以 node 来处理该文件

```
#!/usr/bin/env node
```

在`package.json`中添加以下代码，表示可执行的文件

```
"bin": "index.js"
```

更新中...

### 关于 window 平台 powershell 出现以下问题解决方案

问题：`Suggestion [3,General]: 找不到命令 xxx，但它确实存在于当前位置。默认情况下，Windows PowerShell 不会从当前位置加载命令。如果信任此命令，请改为键入“.\xxx”。有关详细信息，请参阅 "get-help about_Command_Precedence"。`，可能是另一种错误提示，解决方法相同

解决：管理员打开 powershell 执行 `Set-ExecutionPolicy RemoteSigned -Scope CurrentUser` [参考](https://docs.microsoft.com/zh-cn/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.1)
