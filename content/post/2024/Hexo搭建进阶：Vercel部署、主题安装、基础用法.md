---
title: Hexo搭建进阶：Vercel部署、主题安装、基础用法
toc: true
tags:
  - 博客
  - Hexo
categories: 博客相关
image: 'https://p.fiveth.cc/img/m/hexo.jpg'
slug: 138e
date: 2024-04-29 21:33:53
---

- 本篇前文：[如何用Hexo搭建个人博客?](/p/bb32.html)
- 本文基于[Hexo博客搭建基础教程(二)](https://www.fomal.cc/posts/4aa2d85f.html)修改，部分转载
- 我的**Hexo优化系列**：[Hexo文章目录](/tags/hexo)

我的博客搭建教程是一年前写的了，非常感谢[fomalhaut](https://www.fomal.cc/)，当时我是跟着他的教程一步步搭建的。最近我终于在b站发了视频版的博客搭建教程，有很多朋友想进一步优化博客。这篇文章接上一篇，教大家将博客部署到vercel、主题安装、Hexo博客的基础用法。

## Vercel部署

Github提供的网址在国内的访问速度较慢，所以推荐在搭建完后用[Vercel](https://vercel.com/)部署，再通过Vercel绑定到自己的域名上，这样会速度会很快。

因为Vercel给我们分配的域名在国内是**无法访问的**，所以我们需要把在Vercel部署的绑定到自己的域名上，这样就可以访问了。

域名可以在[阿里云](https://www.aliyun.com/)等域名供应商购买，我用的是[西部数码](https://www.west.cn/)，便宜好用。

> 建议选用`com`，`cn`或`cc`等常用好记的顶域，对SEO比较友好，自定义部分的长度尽可能短，别人才会更容易地记住你的网站，域名就是网站的名片。

### 使用Vercel部署

1. 首先需要一个[Vercel](https://vercel.com/)账号，这里**推荐用GitHub账户关联，这样你就可以在vercel中直接托管你的GitHub库中的项目了**，实现开发部署一步到位（网络不流畅可以考虑挂梯子）。
2. 当你用你的Github账户关联并绑定手机号登录之后，点击右上角的`Add New Project`创建新的项目，之后导入选项那里选择`Continue with Github`，这时候应该能看到你Github账号的仓库，选择你刚刚部署成功的存储静态博客的仓库`<username>.github.io`右边的`Import`选项，表示你要导入该仓库。
3. 起一个只能有字母、数字或者或者连字符的项目名称，然后其他默认，点击`Deploy`，等待一分钟即可部署成功，部署成功后电极`Continue to Dashboard`跳转到控制面板，下图所示就是控制面板，看到就代表成功部署到了，但是我们现在还不能访问他给出的域，因为GFW把Vercel屏蔽了。

### 绑定自定义域名

现在你有一个github托管的网址`username.github.io`，以及在Vercel上有一个`blog-demo-chi.vercel.app`，但是它国内无法访问，所以这时候我们就需要将Vercel部署的网页绑定到自己的域名上。

**接下来我们绑定自己的域名：**

1. 点击Vercel控制面板右上角的`View Domains`查看当前的域，我们可以看到仅有Vercel给你预分配的一个域名，此时我们输入我们要用的域名，添加后他会提示你添加一条DNS解析记录。
> 我买的域名是`fiveth.cc`，我决定用二级域名`blog.fiveth.cc`当我博客的域名，大家也可以直接用根域名
2. 接下来在域名解析记录里面添加记录，其中记录类型对应`Type`，主机记录对应`Name`，记录值对应`Value`，其他的设置默认即可。
> 如果不成功，可以尝试统一用`A记录`，值`76.223.126.88`
>
> 如下是我的示例，`blog`解析出来的域名是`blog.fiveth.cc`，`@`解析出来的是根域名`fiveth.cc`

<img src="https://p.fiveth.cc/img/2024/0429203503.png" style="zoom:33%;" />

3. 回到Vercel刚刚查看域名的地方，如果操作没问题，应该会显示域名配置成功的提示，此时就可以通过自定义域名来访问我们搭建的网站了。
4. 当你有了新的域名之后，需要将`[BlogRoot]\_config.yml`文件中的`url`配置项改为自己的新域名，这样博客的文章链接才会正确生成。

## 安装主题

### Butterfly主题

[Butterfly主题](https://butterfly.js.org/)是最流行的主题，拥有很大的社区，自定义度高，还可以用来魔改。

- 这是官方的安装文档：[【Butterfly 安裝文檔】](https://butterfly.js.org/posts/21cfbf15)

- Butterfly的魔改：推荐文章[【博客魔改教程总结(一)】](https://www.fomal.cc/posts/eec9786.html)

### 安知鱼主题

[安知鱼主题](https://docs.anheyu.com/)一款基于Butterfly主题修改的主题，也是我目前在使用的。

如果大家也想用和我一样的话，可以直接跟着[【安知鱼主题官方文档】](https://docs.anheyu.com/)进行安装和配置，文档非常详细。

### 其他主题

hexo还有很多其他主题，可以在[hexo主题](https://hexo.io/themes/)中找到。

## 基础用法

### Front-matter

`Front-matter` 是 markdown 文件最上方以`---`分隔的区域，用于指定个别档案的变数。

- Page Front-matter 用于页面配置
- Post Front-matter 用于文章页配置

如果标注可选的参数，可根据自己需要添加，不用全部都写

**Page Front-matter：**

```MARKDOWN
---
title:
date:
updated:
type:
comments:
description:
keywords:
top_img:
mathjax:
katex:
aside:
aplayer:
highlight_shrink:
---
```

| 写法             | 解释                                                                          |
| :--------------- | ----------------------------------------------------------------------------- |
| title            | 【必需】页面标题                                                              |
| date             | 【必需】页面创建日期                                                          |
| type             | 【必需】标籤、分类和友情链接三个页面需要配置                                  |
| updated          | 【可选】页面更新日期                                                          |
| description      | 【可选】页面描述                                                              |
| keywords         | 【可选】页面关键字                                                            |
| comments         | 【可选】显示页面评论模块(默认 true)                                           |
| top_img          | 【可选】页面顶部图片                                                          |
| mathjax          | 【可选】显示mathjax(当设置mathjax的per_page: false时，才需要配置，默认 false) |
| kates            | 【可选】显示katex(当设置katex的per_page: false时，才需要配置，默认 false)     |
| aside            | 【可选】显示侧边栏 (默认 true)                                                |
| aplayer          | 【可选】在需要的页面加载aplayer的js和css,请参考文章下面的音乐 配置            |
| highlight_shrink | 【可选】配置代码框是否展开(true/false)(默认为设置中highlight_shrink的配置)    |

**Post Front-matter：**

```MARKDOWN
---
title:
date:
updated:
tags:
categories:
keywords:
description:
top_img:
comments:
image:
toc:
toc_number:
toc_style_simple:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
mathjax:
katex:
aplayer:
highlight_shrink:
aside:
---
```

| 写法                  | 解释                                                                                      |
| --------------------- | ----------------------------------------------------------------------------------------- |
| title                 | 【必需】文章标题                                                                          |
| date                  | 【必需】文章创建日期                                                                      |
| updated               | 【可选】文章更新日期                                                                      |
| tags                  | 【可选】文章标籤                                                                          |
| categories            | 【可选】文章分类                                                                          |
| keywords              | 【可选】文章关键字                                                                        |
| description           | 【可选】文章描述                                                                          |
| top_img               | 【可选】文章顶部图片                                                                      |
| cover                 | 【可选】文章缩略图(如果没有设置top_img,文章页顶部将显示缩略图，可设为false/图片地址/留空) |
| comments              | 【可选】显示文章评论模块(默认 true)                                                       |
| toc                   | 【可选】显示文章TOC(默认为设置中toc的enable配置)                                          |
| toc_number            | 【可选】显示toc_number(默认为设置中toc的number配置)                                       |
| toc_style_simple      | 【可选】显示 toc 简洁模式                                                                 |
| copyright             | 【可选】显示文章版权模块(默认为设置中post_copyright的enable配置)                          |
| copyright_author      | 【可选】文章版权模块的文章作者                                                            |
| copyright_author_href | 【可选】文章版权模块的文章作者链接                                                        |
| copyright_url         | 【可选】文章版权模块的文章连结链接                                                        |
| copyright_info        | 【可选】文章版权模块的版权声明文字                                                        |
| mathjax               | 【可选】显示mathjax(当设置mathjax的per_page: false时，才需要配置，默认 false)             |
| katex                 | 【可选】显示katex(当设置katex的per_page: false时，才需要配置，默认 false)                 |
| aplayer               | 【可选】在需要的页面加载aplayer的js和css,请参考文章下面的音乐 配置                        |
| highlight_shrink      | 【可选】配置代码框是否展开(true/false)(默认为设置中highlight_shrink的配置)                |
| aside                 | 【可选】显示侧边栏 (默认 true)                                                            |

注意：我的博客根目录路径为 【D:/Blog/】，下文所说的根目录都是此路径，将用[BlogRoot]代替。

### 标签页

1. 前往你的Hexo博客根目录，打开`Git Bash`执行如下命令：

   ```SHELL
   hexo new page tags
   ```
   
2. 在`[BlogRoot]\source\`会生成一个含有`index.md`文件的`tags`文件夹。

3. 修改`[BlogRoot]\source\tags\index.md`，添加`type: "tags"`。

   ```MARKDOWN
   ---
   title: tags
   date: 2022-10-28 12:00:00
   type: "tags"
   ---
   ```

### 友情链接

1. 前往你的Hexo博客根目录，打开cmd命令窗口执行如下命令：

   ```shell
   hexo new page link
   ```
   
2. 在`[BlogRoot]\source\`会生成一个含有`index.md`文件的`link`文件夹

3. 修改`[BlogRoot]\source\link\index.md`，添加`type: "link"`

   ```markdown
   ---
   title: link
   date: 2024-4-29 12:00:00
   type: "link"
   ---
   ```
   
4. 前往`[BlogRoot]\source\_data`创建一个`link.yml`文件（如果沒有 `_data` 文件夹，请自行创建），并写入如下信息（根据你的需要写）：

   ```yaml
   - class_name: 1.技术支持
     class_desc: 本网站的搭建由以下开源作者提供技术支持
     link_list: 
       - name: Hexo 
         link: https://hexo.io/zh-cn/
         avatar: https://d33wubrfki0l68.cloudfront.net/6657ba50e702d84afb32fe846bed54fba1a77add/827ae/logo.svg
         descr: 快速、简单且强大的网志框架
         siteshot: https://source.fomal.cc/siteshot/hexo.io.jpg
         
   - class_name: 2.友情链接
     class_desc: 一些好朋友~~
     link_list:
       - name: Fiveth
         link: https://blog.fiveth.cc/
         avatar: https://p.fiveth.cc/avatar.jpg
         descr: 分享知识与生活
         siteshot: https://p.fiveth.cc/siteshot.jpg
   ```
   
   class_name和class_desc支持 html 格式，如不需要，也可以留空。

### 子页面

子页面也是普通的页面，可以使用`hexo new page xxx`创建页面。

### 404页面

主題內置了一个简单的404页面，可在设置中开放。

如需本地预览，请访问 http://localhost:4000/404.html

```yaml
## A simple 404 page
error_404:
  enable: true
  subtitle: "页面沒有找到"
  background: 
```

------

