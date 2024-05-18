---
title: 【Qt C++】Fighter Duck
tags:
  - 游戏
  - C++
  - Qt
categories: 我的项目
image: 'https://p.fiveth.cc/img/m/fighterduck.jpg'
slug: f1e0
date: 2023-04-15 00:00:00
---

最近在学习Qt，做了个游戏当练手项目。

断断续续大概写了两周，程序源码放在文章底部了。

# 游戏美术

用软件Aseprite自己绘制。Aseprite是一款专门针对像素画的软件，界面简洁，适合快速上手，没有ps那些繁琐的功能。

游戏动态背景用ps做成，因为ps的动画功能可以实现流畅的帧变换。

<img src="https://p.fiveth.cc/img/m/drawduck.webp" style="zoom: 25%;" />

# 游戏代码

接下来是如何用Qt提供的接口来实现一些游戏的基本功能

<img src="https://p.fiveth.cc/img/m/duckgame.webp" style="zoom: 50%;" />

## 动画

游戏人物的动态用的是gif动图

用QLabel和QMovie实现

```c++
	QLabel *duck=new QLabel(this);
	QMovie *duck_stand=new QMovie("Images/duck_stand.gif");
	duck->setMovie(duck_stand);
	duck_stand->start(); //开始播放
	duck->setFixedSize(150,150); //设置大小
    duck->setScaledContents(true); //自动大小匹配
	duck->move(x,y); //移动位置

```

## 图片

显示静态图片用QLabel和QPixmap

```C++
	QLabel *duck=new QLabel(this);
	QPixmap duck_stand;
	duck_stand.load("Images/duck_stand.gif");
	QLabel->setMovie(duck_stand);
```

## 事件处理

在head文件中加入，再在源文件中定义

```c++
protected:
    void keyPressEvent(QKeyEvent* e);
    void keyReleaseEvent(QKeyEvent* e);
```

## 游戏主循环

控制帧率，一秒循环多少次

```c++
    QTimer *timer=new QTimer;
    connect(timer,&QTimer::timeout,this,&GameEngine::MainGame); //MainGame()是循环的函数
    timer->start(1000/FRAME); //FRAME帧率
```

## Delay函数

Qt中要使用delay函数可以自己定义，代码如下。

```C++
void delay(int msec)
{
    QTime dieTime= QTime::currentTime().addMSecs(msec);
    while( QTime::currentTime() < dieTime )
    QCoreApplication::processEvents(QEventLoop::AllEvents, 100);
}
```

## 显示文字

```C++
	QLabel *health=new QLabel(widget);
	string_health_number= QString::number(health_number, 10); //将int类型转化成QString
	health->setText(string_health_number);
	health->setFont(QFont("Microsoft YaHei", 20, QFont::Bold));
	health->setStyleSheet("color:red;");
```

<img src="https://p.fiveth.cc/img/m/fighterduck.webp" style="zoom: 30%;" />

# 源码

源码：https://github.com/kevinwu06/FighterDuck
