---
title: WSL 通过SSH登录
date: 2019-05-17 13:24:40
categories: [learn]
tags: [tips]
---

## 配置ssh server

自带的ssh server不好用（好不好用不知道，反正网上的教程都是这么说的），先卸载再安装即可。

```bash
// 卸载
sudo apt-get remove openssh-server
// 安装
sudo apt-get install openssh-server
// 编辑配置文件
// vim /etc/ssh/sshd_config

    Port 22222  # 默认的是22，但是windows有自己的ssh服务用的也是22端口，修改一下
    PubkeyAuthentication yes
    PermitRootLogin prohibit-password

// 重启ssh服务
sudo service ssh --full-restart
```

## 生成公钥秘钥

`xshell` > `工具` >  `新建用户秘钥生成向导` > 下一步，具体如图。

1. 第一步
![img-20190517135954](http://img.xiaoz.site/20190517135954.png)

<!-- more -->

2. 第二步，默认即可
![20190517140458.png](http://img.xiaoz.site/20190517140458.png)

3. 第三步，记住你的密码
![20190517140758.png](http://img.xiaoz.site/20190517140758.png)

4. 第四步，生成密钥成功，这里展示的是公钥，点击保存为文件,，比如`d:\download\key.pub`
![20190517140905.png](http://img.xiaoz.site/20190517140905.png)

## 上传公钥到server

目标地址是/root/.ssh/authorized_keys文件，没有则新建

```bash
cd /root
mkdir .ssh
mv /mnt/d/download/key.pub /root/.ssh
cat key.pub > authorized_keys
```

wsl可以直接访问到windows的文件，在 `/mnt` 目录下

## 设置文件权限

这一步很重要，之前看的有些教程没有写这一步，导致一直连接不上

```bash
chmod 600 authorized_keys
chmod 700 ~/.ssh
```

## 重启ssh

```bash
sudo service ssh --full-restart
```

## xshell 连接ssh

1. 第一步，填写主机和端口
![20190517141621.png](http://img.xiaoz.site/20190517141621.png)

2. 第二步，设置验证方式，选择 `Public Key`，选择相应秘钥，输入之前设置的密码
![20190517141736.png](http://img.xiaoz.site/20190517141736.png)

3. 第三步，点击连接，搞定
![20190517142018.png](http://img.xiaoz.site/20190517142018.png)

## 参考

[使用xshell登录ubuntu on windows(wsl)](https://www.jianshu.com/p/039411d2c1f6)

[WSL 通过SSH登录](http://www.mixoo.cn/2018/03/26/wsl-ssh-login/)