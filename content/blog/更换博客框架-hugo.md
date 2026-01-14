---
title: 关于博客框架迁移
toc: true
tags:
  - 博客
  - Hexo
categories: 博客相关
slug: hugo
image: https://p.fiveth.cc/img/2024/0518131750.webp
date: 2024-05-17
---

hi，大家应该发现了，我的博客变样了

因为我把博客从`Hexo`迁移到了`Hugo`

> - 2022年疫情上网课的时候，我用`Hexo`搭建了我的个人博客
> 
> - 2024将博客迁移至了`Hugo`

------

## 原因

### 优点

1. 相比`Hexo`，`Hugo`的静态`生成速度更快`。在本地运行后半秒不到就能在本地生成完页面，方便调试。
2. 最主要是为了`减少折腾`，这让我可以`专心地写文章`。不再反复折腾博客页面。

### 缺点

1. `Hugo`在国内的`社区`没有`Hexo`的大，意味着遇到问题比较难找到解决方法，需要一定Debug能力
2. 没有`Hexo`那么多`插件功能`

### 所以

大家可以根据个人需求和喜好选择，看看自己是要简约的，还是多功能、自定义化高的。

> 这里奉上我的[Hexo博客搭建教程](https://blog.fiveth.cc/tags/hexo/)

------

## 关于Hugo安装

这里列出我在hugo博客搭建过程中阅读的文章：

>- [【L1nSn0w】(1)带着Stack主题入坑Hugo](https://blog.linsnow.cn/p/join-hugo-and-stack/)
>
>- [【L1nSn0w】(2)部署你的Hugo博客](https://blog.linsnow.cn/p/deploy-hugo/)
>
>- [【CSDN】GitHub Action自动化部署Hugo博客](https://blog.csdn.net/m0_51993913/article/details/132657065)
>
>- [【L1nSn0w】(3)Stack主题的自定义](https://blog.linsnow.cn/p/modify-hugo/)


**我在自动部署处做了些改动**：
把`deploy_key`那行换成`PERSONAL_TOKEN: ${{ secrets.PERSONAL_TOKEN }}`，然后在仓库设置添加secret变量`PERSONAL_TOKEN`，里面填`token`。`token`在用户设置中创建，要勾选`repo`和`workflow`权限。

