---
title: Commander.js
date: 2021-01-21 10:25:13
categories:
tags:
---

[Commander.js](https://github.com/tj/commander.js) 是用来搞 node.js 命令行的，现在通过一个简单的例子来学习如何使用 Commander.js

## 需求

做一个简单的创建文件的命令行，输入个数、前缀和后缀，创建文件或文件夹

## todo

- [ ] 可以根据参数批量创建文件
- [ ] 利用 `Inquirer.js` ，实现交互式命令行

## 初始化项目

既然是用于创建文件的，那我们就叫做 `mkf-cli` 吧，在你的命令行工具依次执行以下命令：

```bash
mkdir mkf-cli
cd mkf-cli
yarn init -y
```

编辑器打开项目后，新建`index.js`文件作为我们的入口文件，文件第一行添加以下代码，意思是告诉工具已 node 来处理该文件

```
#!/usr/bin/env node
```

在`package.json`中添加以下代码

```
"bin": "index.js"
```

更新中...

### 关于 window 平台 powershell 出现以下问题解决方案

问题：`Suggestion [3,General]: 找不到命令 xxx，但它确实存在于当前位置。默认情况下，Windows PowerShell 不会从当前位置加载命令。如果信任此命令，请改为键入“.\xxx”。有关详细信息，请参阅 "get-help about_Command_Precedence"。`

解决：管理员打开 powershell 执行 `Set-ExecutionPolicy RemoteSigned -Scope CurrentUser` [参考](https://docs.microsoft.com/zh-cn/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.1)
