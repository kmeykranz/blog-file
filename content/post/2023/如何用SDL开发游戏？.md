---
title: 如何用SDL开发游戏？
tags:
  - SDL2
  - 游戏
  - C++
categories: 代码编程
image: 'https://p.fiveth.cc/img/m/sdl.jpg'
slug: b01b
date: 2023-01-25 00:00:00
---

SDL（Simple DirectMedia Layer）是一套开放源代码的**跨平台多媒体开发库**，使用C语言写成。SDL提供了数种控制图像、声音、输出入的函数，让开发者只要用相同或是相似的代码就可以开发出跨多个平台的应用软件。现SDL多用于开发游戏、模拟器、媒体播放器等多媒体应用领域。

最近前段时间浅学习了下SDL，以及做游戏。这篇文章总结下。

## 游戏结构

做游戏这里最主要的是**面向对象**编程思想。

**多创建类文件**可以帮助我们更好的理清逻辑，管理代码。

整个游戏的结构：

```cpp
Init();//初始化
while(Running)//运行状态循环
{
Events();//事件
Loop();//循环
Render();//渲染
}
Clean();//清除
```

这样把整个游戏结构写成函数形式可以让我们更好的管理代码。这应该就是我们main函数大概的样子。

## SDL函数

接下来我们来看看SDL给我们提供的函数，让我们怎么去实现游戏功能。

首先我们来看看一个简单的SDL**创建窗口**的程序。

```cpp
int main(int,char**) {
	if (SDL_Init(SDL_INIT_VIDEO)) {
		SDL_Log("Cannot Init Video,%s", SDL_GetError());
		return 1;
	}
	SDL_Window* win = SDL_CreateWindow(
		"Title",
		SDL_WINDOWPOS_CENTERED,
		SDL_WINDOWPOS_CENTERED,
		400, 300, SDL_WINDOW_SHOWN
	);
	if (win == NULL) {
		SDL_Log("Cannot Create Window,%s",SDL_GetError());
		return 1;
	}
	SDL_Delay(3000); //延迟3000毫秒

	SDL_DestroyWindow(win);
	SDL_Quit();
	return 0;
}
```

### 检测故障

我们可以看到我们在初始化和创建的时候**会写一个判断语句**。

这是为什么？

因为有时可能会因为各种原因导致**初始化失败**或者**创建失败**，到后面代码多了如果某个环节失败了，我们想要找出问题所在会非常困难。

所以我们在初始化和创建的时候写判断语句，比如第一个**SDL_Init()**，这个SDL给出的函数如果失败了会返回1(可以右键进入定义代码查看)，所以如果返回了1，我们就用**SDL_Log()**函数(SDL给出的输出函数,也可以用cout或者printf)输出问题，后面的**SDL_Error()**可以获取问题。

这样的检测故障的判断是必须的。

### 清除

可以看到我们最后使用了

	SDL_DestroyWindow(win);
	SDL_Quit();

这是官方给出的销毁/清除的函数。

在我们初始化了SDL的功能后，在程序末尾我们必须用SDL_Quit()将其清除。

同样道理，我们用SDL创建的数据变量也应该销毁/释放掉。

(每个数据变量都有对应的销毁/释放函数，如SDL_DestroyRenderer,SDL_FreeSurface)

## SDL学习

SDL给出了非常非常多的功能函数。

大家使用时应该学会查看官方文档，里面都给出了详细的使用方法。

这是我之前学习SDL的文件，里面有一些最基本功能的实现，大家可以看看。



[SDL学习文件 蓝奏云](https://kevinwu06.lanzout.com/i3Stw0lrzw4f)
