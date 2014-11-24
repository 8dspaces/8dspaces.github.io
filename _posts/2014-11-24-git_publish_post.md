---
layout: post
category : blog
tagline: ""
tags : [git]
---
{% include JB/setup %}

## 用Git发布文章

jekyll 搭建博客之后（基于github)，在本地写好文章  
那么问题来了：怎么发布到github上呢？  
当然你可以通过github 的界面（web/客户端）完成提交， 那么有没有更geek点的方式呢？

 参考：[如何搭建Jekyll博客](http://www.mceiba.com/develop/jekyll-introduction.html)

有，那就是直接通过git 连接到github, 完成所有change的提交

> 大多数 Git 服务器都会选择使用 **SSH 公钥**来进行授权。系统中的每个用户都必须提供一个公钥用于授权，
> 没有的话就要生成一个。生成公钥的过程在所有操作系统上都差不多。 首先先确认一下是否已经有一个公钥了。
> SSH 公钥默认储存在账户的主目录下的 ~/.ssh 目录。进去看看：

windows下会类似 `C:\users\xxx\.ssh\`

        $ cd ~/.ssh
        $ ls
        authorized_keys2  id_dsa       known_hosts
        config            id_dsa.pub
        
id_dsa 和 id_dsa.pub 就是需要的文件  
有 .pub 后缀的文件就是公钥，另一个文件则是密钥


### 生成SSH Key 与github 建立连接 

可以用 ssh-keygen 来创建。该程序在 Linux/Mac 系统上由 SSH 包提供，而在 Windows 上则包含在 MSysGit 包里：

        $ ssh-keygen
        Generating public/private rsa key pair.
        Enter file in which to save the key (/Users/schacon/.ssh/id_rsa):
        Enter passphrase (empty for no passphrase):
        Enter same passphrase again:
        Your identification has been saved in /Users/schacon/.ssh/id_rsa.
        Your public key has been saved in /Users/schacon/.ssh/id_rsa.pub.
        The key fingerprint is:
        43:c5:5b:5f:b1:f1:50:43:ad:20:a6:92:6a:1f:9a:3a schacon@agadorlaptop.local
        
它先要求你确认保存公钥的位置（.ssh/id_rsa），然后它会让你重复一个密码两次，如果不想在使用公钥的时候输入密码，可以留空。

有了公钥之后，想连到github(git 服务器）就可以在你的github 上加一个SSH key,名字随意
 .pub 文件的内容然后发邮件给管理员。公钥的样子大致如下：

        $ cat ~/.ssh/id_rsa.pub
        ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAklOUpkDHrfHY17SbrmTIpNLTGK9Tjom/BWDSU
        GPl+nafzlHDTYW7hdI4yZ5ew18JH4JW9jbhUFrviQzM7xlELEVf4h9lFX5QVkbPppSwg0cda3
        Pbv7kOdJ/MTyBlWXFCR+HAo3FXRitBqxiX1nKhXpHAZsMciLq8V6RjsNAQwdsdMFvSlVK/7XA
        t3FaoJoAsncM1Q9x5+3V0Ww68/eIFmb1zuUFljQJKprrX88XypNDvjYNby6vw/Pb0rwert/En
        mZ+AW4OZPnTPI89ZPmVMLuayrD2cE86Z/il8b+gw3r3+1nKatmIkjn2so1d01QraTlMqVSsbx
        NrRFi9wrf+M7Q== schacon@agadorlaptop.local

这样通过本机的git就可以连接到github上的代码仓库了

### 提交change 到github 

        # 克隆你的博客到本地
        git clone https://github.com/<username>/<repository>.git 
        
        # 如已经clone过了，可以用pull拿到最新版本
        git pull
        
        # 更改之后，简单点加更改到Header（提交）
        git add .  (也可以 git add -A)
        # 正规点为以后便于检索，打个label 
        git commit <label name>
        
        # 提交
        git push
        
当然这是最最简单的git操作，参考 [GIT操作小结](http://wklken.me/posts/2013/12/01/git-base.html)
