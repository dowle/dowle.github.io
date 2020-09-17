---
title: "MAC git仓库多key配置"
date: 2020-09-17T18:25:47+08:00
tags: ["git", "ssh"]
categories: ["CVS"]
draft: false
---
## 前言  
SSH 是连接远程主机最常用的方式，尽管连接到单个主机的基本操作非常直接，但当你开始使用大量的远程系统时，这就会成为笨重和复杂的任务。

幸运的是，OpenSSH 允许您提供自定义的客户端连接选项。这些选项可以被存储到一个配置文件中，这个配置文件可以用来定义每个主机的配置。这有助于保持每个主机的连接选项更好的独立和组织，也你让你在需要连接时避免在命令行中写繁琐的选项。  

配置文件的位置: ~/.ssh/config  

可以用来设置多个平台的git私钥。  
## 配置说明
- Host  
用来指定该key的Host名字，此处必须使用本地repo的hostname github-A。Host必须跟repo的hostname保持一致，也就是git到时候会用自己repo的hostname来ssh配置文件里面找是不是有对应的Host，找到了就使用该配置，具体访问的域名会采用HostName  
- Hostname xx  
此处指定Host对应的具体域名  
- User git  
说明该配置的用户得是git  
- Port 22  
配置端口  
- IdentityFile /Users/xxx/.ssh/id_rsa.github  
这行最为关键，指定了该使用哪个ssh key文件，这里的key文件一定指的是私钥文件。  
## 示例
```sh
# 跳板机配置 -- 配置了，使用ssh时不用指定下面这些参数, 直接 ssh jumpsercer即可使用
Host  jumpserver
    HostName        xxxxxx.com
    Port            2222
    User            dudw
    IdentityFile    ~/.ssh/id_rsa
    TCPKeepAlive    yes
    ForwardAgent    yes
    StrictHostKeyChecking no
    UserKnownHostsFile    /dev/null
    ControlMaster   auto
# github的git私钥
Host github.com
    HostName       github.com
    IdentityFile   ~/Desktop/key/common
# gitee的git私钥
Host gitee.com
    HostName       gitee.com
    IdentityFile   ~/Desktop/key/common
```

git可以使用 ssh -T git@github.com 测试key是否生效。
