---
title: Medium解锁：用脚本一键解锁Medium付费文章
slug: medium-unlock
date: 2025-01-09
categories: 经验分享
tags:
  - 干货
  - 阅读
toc: true
image: https://p.fiveth.cc/img/2025/0109193039.png
---
Medium 是外网一个阅读文章的平台，他的特点是**文章质量高**，在里面可以学到很多知识，涉及各种领域，非常适合日常浏览，打破信息差。但是很多的高质量的内容需要订阅会员才能阅读。这篇文章就给大家提供一个白嫖的方法。

![image.png](https://p.fiveth.cc/img/2025/0109174219.png)

我们要用到的破解网站是 [ReadMedium](https://readmedium.com/)，在这个网站内填入 Medium 的付费文章链接，就可以自动获取隐藏的文章内容，进行阅读。网站还提供了一个方法，就是在 medium 的文章页面，把域名改成 readmedium.com 就可以跳转。网站还提供了一个 AI 概览的功能。

![image.png](https://p.fiveth.cc/img/2025/0109174335.png)

但是这样还是略显麻烦，所以我们可以用油猴上的一个脚本 [Medium-Unlock](https://greasyfork.org/zh-CN/scripts/481493-medium-unlocker-2023-12-06-updated) ，它会在 medium 页面上加入一个快捷按钮，点击就会自动跳转到对应的 readmedium 页面。

接下来，我会教大家怎么安装这个脚本，以及如何在 ios 系统 Safari上使用这个脚本，这样我们在手机上也可以使用。

---
## 在Chrome上安装

>  这里演示 `chrome 浏览器`（edge 同理），首先打开谷歌应用商店，然后搜索 ` Tampermonkey `  安装油猴扩展。

![image.png](https://p.fiveth.cc/img/2025/0109174736.png)

![image.png](https://p.fiveth.cc/img/2025/0109174857.png)


>  然后我们打开这个链接 [Medium-Unlock](https://greasyfork.org/zh-CN/scripts/481493-medium-unlocker-2023-12-06-updated) ，点击安装脚本，然后在弹出的页面再次点击安装即可。

---

## IOS系统在Safari上安装

要在 Safari 上安装拓展，我们要先安装这个软件：`Userscript `

![35999d1fb9c881cfae2615d677ad834.jpg](https://p.fiveth.cc/img/2025/0109175638.jpg)


>  打开后，我们点击 change directory，在文件中新建一个文件夹用于储存脚本，名称随意。

>  接下来，我们到 `设置->App->扩展 ` 中打开 `userscript` 的权限。

![df532cf29104145e27ef0bd0112d777.jpg](https://p.fiveth.cc/img/2025/0109175952.jpg)

>  然后打开浏览器，在上方扩展按钮，点管理拓展，然后打开 `userscript`

![fd0a2a307f4b787901a94bd2b3ba0c3.jpg](https://p.fiveth.cc/img/2025/0109175957.jpg)

现在我们再打开 medium，就可以看到旁边的解锁按钮啦。