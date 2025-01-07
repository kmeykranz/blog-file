---
title: 摆脱3DM启动器打开游戏：避免捆绑下载笑傲江湖
toc: true
tags:
  - 游戏
  - 干货
categories: 教学干货
image: 'https://p.fiveth.cc/img/m/3dm.jpg'
slug: 134b
date: 2023-06-06 22:15:15
---

经常白嫖游戏的小伙伴应该都对[3DM](https://www.3dmgame.com/)不陌生，里面有很多盗版游戏资源。但是呢，很多3dm下来的游戏都要用3dm启动器去打开，你说用你启动器打开就算了，就多点两下的事对吧。但是每次打开你启动器，你都要下载个“笑傲江湖”到我电脑里，删了还没用，下次启动的时候又会跳出来，这跟流氓软件有什么区别。今天就教大家怎么摆脱3DM流氓启动器，直接打开游戏。

<img src="https://p.fiveth.cc/img/m/xiaoao.webp" />

首先我们打开3dm启动器目录，如图，打开`3dmConfig.ini`文件，找到游戏exe文件的路径。

<img src="https://p.fiveth.cc/img/m/3dmpath.webp"/>

接下来我们顺着路径去寻找，会发现根本就找不到游戏的exe文件。别慌，这是3dm耍的小把戏，把游戏启动的exe文件给隐藏了。我们要做的就是把被隐藏的文件给显示出来。

我们打开cmd窗口，进入对应的目录，然后执行dos命令：

> 如何打开cmd窗口？win+r，然后输入cmd，点确定

```
attrib -s -a -h -r wwzRetailEgs.exe
```

<img src="https://p.fiveth.cc/img/m/cmd.webp"/>

然后我们就能看到游戏的exe文件在目录中显示出来了，我们可以右键然后将快捷方式创建到桌面，之后我们就不需要用3dm启动器来打开游戏了😎。

<img src="https://p.fiveth.cc/img/m/3dmexe.webp"/>
