---
title: 如何用Hexo搭建个人博客?
toc: true
date: 2022-12-10
tags:
  - 博客
  - Hexo
categories: 博客相关
image: 'https://p.fiveth.cc/img/m/hexo.jpg'
slug: bb32
---
> `2024.4` 我出了视频版教程啦：[bilibili视频链接](https://www.bilibili.com/video/BV1Ju4m1c7WR/)

我的个人博客搭建成功啦！

这篇文章会给大家分享我是如何从0开始搭建我的个人独立博客的

接下来我们开始吧

**文章比较精简，没有废话，不要跳步**

## 准备工具

首先我们需要到对应网站下载需要的工具

**[下载nodejs](https://nodejs.org/en/)**

****

*(这篇文章里有修改nodejs缓存路径的教学:[文章链接](https://www.cnblogs.com/liuqiyun/p/8133904.html)，c盘战士可以不看)*

**[下载git](https://git-scm.com/downloads)**

***

一直点确定就行，全部按它默认勾的

*(这里有一篇详细安装教程[文章链接](https://www.cnblogs.com/xueweisuoyong/p/11914045.html)，可以不看)*

**接下来我们测试下是否都下载成功**

管理员运行cmd，依次输入

```
node -v
npm -v（这个是node附带的）
git -v
```

**下载hexo**

```
npm install hexo-cli -g
```

## 搭建仓库

成功下载好工具之后，我们开始搭建GitHub存储静态页面的仓库

首先注册/登入[Github](https://github.com/)

点击Create a new repository进入新建仓库页面

仓库名输入：

```
用户名.github.io
```

勾选 Public

勾选 Add a README file

拉到下面点击create创建

## 生成SSH Keys

进入任意文件夹，右键空白处然后点Git bash here,输入

```
ssh-keygen -t rsa -C "邮件地址"
```

然后敲4次Enter⌨️

然后进入C:\Users\用户名，在里面进入.ssh文件

用记事本打开里面的id_rsa.pub,全选复制里面的代码

**然后打开github**

进入用户设置，找到SSH keys

新建SSH keys，名称随意，在下面粘贴代码，

然后创建

**测试是否成功**

在git bash中输入

```
ssh -T git@github.com
```

回车，然后再输入yes

## 本地生成博客内容

在喜欢位置新建文件Blog，然后进入文件夹

右键空白处然后点Git bash here，输入

```
hexo init
```

> 如果‘command not find’，就在前面加上npx，如：`npx hexo init`

然后

```
hexo install
```

接下来依次输入

```
hexo g
hexo s
```

（如果不成功的话就重复直到成功，因为国内与github连接不稳定）

现在就可以复制生成的链接进入浏览器看到我们生成的本地服务器了

然后回到命令行，ctrl+c关闭

## 上线博客

进入之前的Blog文件夹，用记事本打开_config.yml

拉到最下面将deploy后面的全删掉，复制粘贴这段

```
  type: git
  repository: 
  branch: main
```

> 注意缩进格式：每行前面都有两个空格不要删，每个冒号后面都有个空格也不要删！

去github之前生成的仓库页面，点code，复制https链接

将其粘贴到我们记事本中的`repository：`后面

然后保存退出

**回到博客文件夹，git bash**

安装自动部署发布工具

```
npm install hexo-deployer-git --save
```

然后在Blog文件夹右键打开git bash，依次输入

```
hexo g（生成）
hexo d（上传）
```

> 如果是第一次使用git的话会需要配置
>
> ```
> git config --global user.email "你的邮箱"
> git config --global user.name "你的名字"
> ```
>
> 配置完后再`hexo d`上传
>
> 在跳出来的窗口内进行登录

接下来我们就成功把本地内容上传到github了

上传成功以后，我们就算搭建好了！上自己的网址看看吧

网址是我们之前设的仓库名：用户名.github.io

## 网站资料

我们的博客标题还是默认的hexo，整个页面是初始默认的，接下来我们对其进行修改

用记事本打开我们blog文件夹中的_config.yml文件

将#Site下面按自己的需求填上

```
## Site
title: 标题
subtitle: 副标题
description: 描述
keywords: 关键词
author: 站主
language: 语言（可以填写zh-CN）
timezone: 时区（可以填写Asia/Shanghai）
```

然后保存

## 如何上传文章

我们在Blog文件夹中打开git bash,输入下方代码就可以生成新的文章md文件

```
hexo new 文章标题
```

文章是.md格式，在我们的Blog文件夹中的source/_posts中

推荐用Typora软件来编辑.md格式的文件

> Typora官网：https://www.typoraio.cn/（89元终身使用，推荐正版）
>
> 破解版奉上：[蓝奏云文件](https://kevinwu06.lanzout.com/iXkq30icv1ha)

然后我们用Typora软件打开该.md文件就可以开始写文章了

写好以后，我们还是一样打开git bash生成、上传

```
hexo g
hexo d
```

------

至此，我们就成功搭建好基本的博客了，剩下的就是对博客的一些优化和美化了。

我目前使用的hexo博客主题是[anzhiyu](http://docs.anheyu.com/)，推荐主题：[Butterfly](https://butterfly.js.org/posts/21cfbf15/)，[anzhiyu](http://docs.anheyu.com/)

大家可以参阅主题文档进行安装配置

有什么问题的话欢迎评论。

{% tip info %}

本篇下文：[Hexo搭建进阶：Vercel部署、主题安装、基础用法](/p/138e.html)

我的**Hexo优化系列**：[Hexo文章目录](https://www.fomal.cc/posts/4aa2d85f.html)

{% endtip %}
