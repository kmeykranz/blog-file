---
title: Hexo博客优化：如何优化Hexo的网站性能？
tags:
  - 博客
  - Hexo
categories: 博客相关
image: 'https://p.fiveth.cc/img/m/perf.jpg'
slug: 95d6
date: 2023-10-11 12:06:31
updated: 2024-4-3
---

对一个网站来说，性能是非常重要的，它决定了用户在网页停留的时长。

如果网页的加载速度很慢的话，用户很容易就会离去。

有朋友说我的博客速度挺快的，这篇文章就讲讲我是如何**优化我的网站性能**的。

# 性能检测

首先我们可以使用网站性能检测工具，来获取网站的性能报告，这样我们就可以知道是哪部分拖慢了网站速度，从而对其进行优化。

我用的工具是**Chrome开发者工具**中自带的**Lighthouse**，给出性能报告的同时，它还会给出一些优化的建议。

<img src="https://p.fiveth.cc/img/m/lighthouse.webp" alt="lighthouse"/>



# Vercel网站托管

我使用的是[Vercel](https://vercel.com/dashboard)来托管我的网站，它在全球以及国内都有非常优秀的速度。如果想要让其在国内可以浏览的话，需要准备一个域名。

> 具体如何使用可以参考[Fomalhaut](https://www.fomal.cc/)的文章[Hexo博客搭建基础教程(二)](https://www.fomal.cc/posts/4aa2d85f.html)中有详细教程。

`注意`：cname的解析方法目前会导致无法在国内访问，需要做一些修改
**失效方法：**记录值类型cname，值为cname.vercel-dns.com
**解决方法：**记录值类型A类型，值为76.223.126.88

<img src="https://p.fiveth.cc/img/m/jiexi.webp" alt="解析记录" />



# Vercel加速节点（新）

{% note info flat %}此章为2024.4新添加内容{% endnote %}

这是一个vercel加速节点，使用是会自动解析至附近可用节点，尽可能的选择优质节点。

**食用方法**：将原来解析至 `cname.vercel.com` 改为 `vercel.cdn.yt-blog.top`（CNAME记录）

>  由[Fgaoxing](https://www.yt-blog.top/)提供，原文链接：[【推一下Vercel加速节点】](https://www.yt-blog.top/9952/)



# 图片压缩

在我们的博客中，最消耗流量的应该就是图片了。所以为了提升加载速度，对图片的压缩非常重要。

1.每次将图片上传到图床之前，我都会对其进行压缩（控制在50kb以下）

2.同时，将图片转换成webp格式也会大大提高加载速度。

> 这里推荐一个我一直在用的[多功能在线图片压缩](https://imagestool.com/webp2jpg-online/)工具，非常方便快捷。



# lazyload

**lazyload(懒加载)**是一个对提升性能非常有效的功能，它会优先加载处于屏幕显示区域内的资源，而不是等待一整个页面的资源加载完毕。这让许多资源可以在用户边浏览的时候边加载，节省时间。

我给我的评论模块开启了lazyload，因为很多时候评论模块加载都会慢，导致用户要在加载页面等待。



# gulp压缩

[gulp插件](https://www.gulpjs.com.cn/)可以帮助我们自动压缩博客静态资源。

> 具体如何安装使用可以参考[Akilar](https://akilar.top/)的文章[【使用gulp压缩博客静态资源】](https://akilar.top/posts/49b73b87/)



# CDN

让所有的静态资源通过**CDN(内容分发网络)**加载，可以提高资源加载速度。

> 我使用的是[HEO](https://blog.zhheo.com/)大佬分享的[【Butterfly CDN链接更改指南】](https://blog.zhheo.com/p/790087d9.html)中给出的cdn

这里给出我在butterfly中的cdn列表：

```yml
    pjax: https://lib.baomitu.com/pjax/0.2.8/pjax.min.js
    twikoo: https://lf6-cdn-tos.bytecdntp.com/cdn/expire-1-M/twikoo/1.4.18/twikoo.all.min.js
    sharejs: https://lib.baomitu.com/social-share.js/1.0.16/js/social-share.min.js
    sharejs_css: https://lib.baomitu.com/social-share.js/1.0.16/css/share.min.css
    lazyload: https://lf3-cdn-tos.bytecdntp.com/cdn/expire-1-M/vanilla-lazyload/17.3.1/lazyload.iife.min.js
    instantpage: https://lf3-cdn-tos.bytecdntp.com/cdn/expire-1-M/instant.page/5.1.0/instantpage.min.js
    typed: https://lf9-cdn-tos.bytecdntp.com/cdn/expire-1-M/typed.js/2.0.12/typed.min.js
    snackbar_css: https://lf3-cdn-tos.bytecdntp.com/cdn/expire-1-M/node-snackbar/0.1.16/snackbar.min.css
    snackbar: https://lf6-cdn-tos.bytecdntp.com/cdn/expire-1-M/node-snackbar/0.1.16/snackbar.min.js
    fontawesome: https://lf6-cdn-tos.bytecdntp.com/cdn/expire-1-M/font-awesome/6.0.0/css/all.min.css
    justifiedGallery_js: https://lf3-cdn-tos.bytecdntp.com/cdn/expire-1-M/justifiedGallery/3.8.1/js/jquery.justifiedGallery.min.js
    justifiedGallery_css: https://lf3-cdn-tos.bytecdntp.com/cdn/expire-1-M/justifiedGallery/3.8.1/css/justifiedGallery.min.css
    aplayer_css: https://lf6-cdn-tos.bytecdntp.com/cdn/expire-1-M/aplayer/1.10.1/APlayer.min.css
    aplayer_js: https://lf6-cdn-tos.bytecdntp.com/cdn/expire-1-M/aplayer/1.10.1/APlayer.min.js
    meting_js: https://npm.elemecdn.com/hexo-anzhiyu-music@1.0.1/assets/js/Meting2.min.js
```



# 总结

这些大概就是我对我的网站做的优化总结了，其实还有很多地方可以再提升，但是对我来说已经够用了。

希望大家都可以积极地去优化网站的性能，这样才可以收获更多的游客🙂



