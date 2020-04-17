---
title: Hexo博客——安装与部署
id: 191106
date: 2019-11-06 15:28:33
tags: hexo
categories: 技术
---

最近花时间搭建了Hexo博客，用来记录一切自己想写的内容，第一个篇正好用来写hexo相关内容。  
原本想用虚拟主机搭个人网站，但考虑到购买费用和运维成本，且目前几乎不需要挂载任何服务，在寻找替代方案过程中遇见了[Hexo](https://hexo.io/zh-cn/)。
Hexo是一款基于Node.js的快速、简洁且高效的免费博客框架，所有成本仅为域名购买和少许的熟悉时间，主题丰富，使用者和教程数不胜数，而且文档有中文版，对于小白十分友好。
<!--more-->  

第一部分：安装与部署
第二部分：博客使用
第三部分：Next主题优化
第四部分：第三方插件引入

## 1.安装
在安装hexo之前需要安装另两个程序：
- [Node.js](https://nodejs.org/en/)
- [Git](https://git-scm.com/)  

在上面链接中下载、安装对应版本就可以开始hexo安装了。
先创建一个文件夹hexo，然后cd到这个文件夹下（或者在这个文件夹下直接右键git bash打开）。输入以下指令：
```
npm install -g hexo-cli  
```

使用hexo -v查看是否安装成功。
接下来初始化hexo
```
hexo init blog
```

blog可以改成任意名字，只要记住hexo安装于此就行。
之后进入blog目录安装npm
```
cd blog //进入这个blog目录
npm install
```
随后blog目录下会生成多个目录。

- node_modules: 依赖包
- public：存放生成的页面
- scaffolds：生成文章的一些模板
- source：用来存放你的文章
- themes：hexo主题
- _config.yml: 博客的配置文件

输入以下命令既可在本地查看生成的博客：
```
hexo cl 
hexo g
hexo s
```
后续修改博客配置后重新生成本地版本都使用上述命令，也可以使用混合版本:
```
hexo cl & g & s 
```

## 2.部署

部署使用github和coding，做到在解析域名后境外线路进入部署在github上的博客，境内线路访问coding上的博客，避免线路延迟。

#### 部署博客到github
下面先介绍[github](https://github.com/login)部署。
登录github账号后创建一个新仓库。
![新建仓库](https://i.loli.net/2019/11/06/GNQsfHiWZbjAezn.png)

仓库名字填写格式为xxx.github.io，其中xxx为你的github用户名，正确填后部署到GitHub page的时才会被识别。MaouLuo为我的用户名，改成自己的就行。
![仓库名填写](https://i.loli.net/2019/11/06/fsFRN3zJq8uDSMH.png)

回到git bash中输入以下命令：
```
git config --global user.name "name"
git config --global user.email "email"
```

其中name部分替换成你的github用户名，email替换成你的github邮箱。
然后创建SSH,一直回车
```
ssh-keygen -t rsa -C "email"
```
email替换同上一步，然后会显示生成的SSH所在路径。
SSH目录中id_rsa是你这台电脑的私人秘钥，id_rsa.pub是公共秘钥，举个不够恰当的栗子，公钥就是首付款的二维码，私钥则是付款密码，需自己藏好。
在github中保存你公钥，右上角头像 > Settings > SSH and GPG keys > New SSH key > key > 粘贴公钥中内容

查看是否保存成功。
```
ssh -T git@github.com 
```

这一步，我们就可以将hexo和GitHub关联起来，也就是将hexo生成的文章部署到GitHub上，打开站点配置文件 _config.yml，翻到最后，修改为
YourgithubName就是你的GitHub账户
```
deploy:
  type: git
  repo: https://github.com/YourgithubName/YourgithubName.github.io.git
  branch: master
```

这个时候需要先安装deploy-git ，也就是部署的命令,这样你才能用命令部署到GitHub。
```
npm install hexo-deployer-git --save
```

然后
```
hexo cl
hexo g
hexo d
```

注意deploy时可能要你输入username和password。

过一会儿输入http://name.github.io后就能查看博客的线上版本。
