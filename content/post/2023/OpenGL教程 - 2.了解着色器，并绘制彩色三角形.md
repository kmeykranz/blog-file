---
title: OpenGL教程 | 2.了解着色器，并绘制彩色三角形
tags:
  - OpenGL
  - C++
  - 图形学
categories: 学习笔记
image: 'https://p.fiveth.cc/img/m/colorcover.jpg'
slug: '9641'
date: 2023-05-28 16:20:00
---

本篇我们会更深入地了解着色器：

1. 学会着色器如何输入输出
2. 学会使用Uniform，绘制会随时间变化颜色的图形
3. 绘制彩色三角形
4. 定义自己的着色器类

本文参考[LearnOpenGL](https://learnopengl.com/)教学网站

> 经验：建议打开源代码看着学习，光跟着文档的话思路会很乱

下面是我做的笔记

配合源码食用更佳🤫：[OpenGL学习源码](https://github.com/kevinwu06/LearnOpenGL)

## 向量

数据类型：一般使用vecn（包含n个float分量的默认向量）

**重组（灵活的分量选择方式）：**

```c++
//重组的例子
vec2 someVec;
vec4 differentVec = someVec.xyxx;
vec3 anotherVec = differentVec.zyw;
vec4 otherVec = someVec.xxxx + anotherVec.yxzy;

vec2 vect = vec2(0.5, 0.7);
vec4 result = vec4(vect, 0.0, 0.0);
vec4 otherResult = vec4(result.xyz, 1.0);
```

## 输入与输出

我们给着色器加上输入和输出，让顶点着色器为片段着色器决定颜色。

#### 顶点着色器

```cpp
#version 330 core
layout (location = 0) in vec3 aPos; // 位置变量的属性位置值为0

out vec4 vertexColor; // 指定一个颜色输出

void main()
{
    gl_Position = vec4(aPos, 1.0); // 注意我们如何把一个vec3作为vec4的构造器的参数
    vertexColor = vec4(0.5, 0.0, 0.0, 1.0); // 把输出变量设置为暗红色
}
```

#### 片段着色器

```cpp
#version 330 core
out vec4 FragColor;

in vec4 vertexColor; // 从顶点着色器传来的输入变量（名称相同、类型相同）

void main()
{
    FragColor = vertexColor;
}
```

现在运行，就可以看到我们成功将颜色由顶点着色器输入到片段着色器中，将三角形的颜色设置成了深红色。

<img src="https://p.fiveth.cc/img/m/redtriangle.png" style="zoom:50%;" />

## Uniform

Uniform是一种从CPU中的应用向GPU中的着色器发送数据的方式

我们在**片段着色器**中声明Uniform

```cpp
#version 330 core
out vec4 FragColor;

uniform vec4 ourColor; // 在OpenGL程序代码中设定这个变量

void main()
{
    FragColor = ourColor;
}
```

现在，我们就可以在渲染循环中去改变三角形颜色了。这里我们用让它随时间变化颜色。

```cpp
float timeValue = glfwGetTime();
float greenValue = (sin(timeValue) / 2.0f) + 0.5f;
int vertexColorLocation = glGetUniformLocation(shaderProgram, "ourColor");
glUseProgram(shaderProgram);
glUniform4f(vertexColorLocation, 0.0f, greenValue, 0.0f, 1.0f);
```

<img src="https://p.fiveth.cc/img/m/uniform.gif" style="zoom:95%;" />

源代码：[Uniform源码](https://github.com/kevinwu06/LearnOpenGL/blob/main/Uniform.cpp)

## 你好，彩色三角形

现在，我们将把颜色数据添加为3个值至vertices数组。我们将把三角形的三个角分别指定为红色、绿色和蓝色。

```cpp
//顶点数据
float vertices[] = {
    // 位置              // 颜色
     0.5f, -0.5f, 0.0f,  1.0f, 0.0f, 0.0f,   // 右下
    -0.5f, -0.5f, 0.0f,  0.0f, 1.0f, 0.0f,   // 左下
     0.0f,  0.5f, 0.0f,  0.0f, 0.0f, 1.0f    // 顶部
};
```

我们让顶点着色器接收颜色值，并输出到片段着色器。

```cpp
#version 330 core
layout (location = 0) in vec3 aPos;   // 位置变量的属性位置值为 0 
layout (location = 1) in vec3 aColor; // 颜色变量的属性位置值为 1

out vec3 ourColor; // 向片段着色器输出一个颜色

void main()
{
    gl_Position = vec4(aPos, 1.0);
    ourColor = aColor; // 将ourColor设置为我们从顶点数据那里得到的输入颜色
}
```

再修改一下片段着色器，让他输入颜色。

```cpp
#version 330 core
out vec4 FragColor;  
in vec3 ourColor;

void main()
{
    FragColor = vec4(ourColor, 1.0);
}
```

现在我们修改着色器的顶点格式。

```cpp
// 位置属性
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 6 * sizeof(float), (void*)0);
glEnableVertexAttribArray(0);
// 颜色属性
glVertexAttribPointer(1, 3, GL_FLOAT, GL_FALSE, 6 * sizeof(float), (void*)(3* sizeof(float)));
glEnableVertexAttribArray(1);
```

然后我们就可以看到一个彩色的三角形😍

<img src="https://p.fiveth.cc/img/m/colorful.webp" style="zoom:50%;" />

三角形会自动将我们给的三个顶点颜色进行渐变，这是在片段着色器中进行的所谓片段插值(Fragment Interpolation)的结果。

源代码：[彩色三角形源码](https://github.com/kevinwu06/LearnOpenGL/blob/main/%E5%BD%A9%E8%89%B2%E4%B8%89%E8%A7%92.cpp)

## 自己的着色器类

管理着色器类是很麻烦的事，所以我们要写一个类来让我们能更轻松的管理。

我们的着色器类用于：

1. 打开着色器文件
2. 编译和链接着色器
3. use用来激活着色器程序
4. set用于设置和查询uniform

**使用方法**

```cpp
Shader ourShader("path/to/shaders/shader.vs", "path/to/shaders/shader.fs");
...
while(...)
{
    ourShader.use();
    ourShader.setFloat("someUniform", 1.0f);
    DrawStuff();
}
```

顶点和片段着色器的文件名可以任意取（推荐用shader.vs和shader.fs，很直观）

源代码：[着色器类源码](https://github.com/kevinwu06/LearnOpenGL/blob/main/shader.h)

------

恭喜你又学完了一篇教程🎉，你正在向目标一步一步地进发。
