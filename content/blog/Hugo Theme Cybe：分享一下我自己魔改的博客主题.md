---
title: Hugo Theme Cybe：分享一下我自己魔改的博客主题
slug: cybe
date: 2025-03-20
categories: 我的项目
tags:
  - 博客
  - Hugo
toc: true
image: https://p.fiveth.cc/img/2025%2F0320083703.png
---
# 💻主题魔改
前段时间我对Stack主题进行了很大的魔改，魔改内容大概如下：
- 将原先的左侧导航栏换成了传统的顶部导航栏
- 在主页添加一个块状头图，内部显示欢迎语
- 右侧边栏添加名片卡片
- 主页文章双栏展示
- 关于页面的自我介绍卡
- 对手机端的优化

这样网站首先是继承了Stack主题的**卡片风格**，以及**加载速度快**的优点。而在整体布局上则是换成了现在**流行的博客网站布局**，充分利用屏幕空间。
# 🔗主题的分享
看到有朋友对我的主题感兴趣，询问我页面的修改问题，于是我就想着发布我自己的这个主题，提供文档，让一些喜欢的朋友可以使用。

> 主题仓库：[Hugo Theme Cybe](https://github.com/kmeykranz/hugo-theme-cybe)

# 👾主题配置文档
下面是主题的安装方法和配置文档：
## 安装
1.删掉默认的配置文件`config.toml`

2.在`blog`根目录打开终端输入以下命令
```bash
git init
#获取主题文件
git submodule add https://github.com/kmeykranz/hugo-theme-cybe/ themes/hugo-theme-cybe
```

3.将获取到的主题文件中的`exampleSite`中的所有文件拷贝到`blog`根目录

4.打开`hugo.yaml`根据自己需要进行配置

## 自定义头像
1. 打开主题文件中的`\assets\img`文件夹
2. 把里面的avatar.png换成你的头像

## 自定义主页侧边栏名片
1. （用任意编辑器）打开主题文件中的`\layouts\partials\widget\author.html`
2. 在里面自行修改对应文字和链接即可

## 自定义”关于“页面
1. 打开主题文件中的`\layouts\page\about.html`
2. 往下划就会找到这段代码，更换里面的文字和链接就可以了
```html
    <!-- ---------在这里自定义关于页面--------- -->
    <!-- 卡片容器 -->
    <div class="author-page-content">
        <!-- 卡片 1：关于我 -->
        ...
        <!-- 卡片 2：我的人格 -->
        ...
        <!-- 卡片 3：我的爱好 -->
        ...
        <!-- 卡片 4：座右铭 -->
        ...
        <!-- 卡片 5：我的技能 -->
        <!-- 技能图标可以从这个网站获取 https://shields.io/ -->
        ...
        <!-- 卡片 6：与我联系 -->
        ...
    <div>
    <!-- ---------在上面自定义关于页面--------- -->
```

## 主页头图文字修改
在主题文件中打开`layouts\index.html`修改

## 自定义页脚
在主题文件中打开`layouts\partials\footer\custom.html`修改

## 其余设置
其余设置请参考原主题文档进行配置 https://stack.jimmycai.com/

## 支持
如遇到问题，可以提交[github issue](https://github.com/kmeykranz/hugo-theme-cybe/issues)。