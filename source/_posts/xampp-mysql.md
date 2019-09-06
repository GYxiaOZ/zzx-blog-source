---
title: xampp-mysql
date: 2019-09-06 14:19:40
categories: [learn]
tags: [tips]
---

## xampp 下 mysql 初始密码的修改

首先，我们点击 xampp 中的 shell 按钮，登陆 `mysql：c:>mysql -u root -p` ，然后密码直接回车。

现在，应该进入 mysql 了，输入下面这条指令：

```sql
mysql>set password for 'root'@'localhost'=password('newpasswd');
```
