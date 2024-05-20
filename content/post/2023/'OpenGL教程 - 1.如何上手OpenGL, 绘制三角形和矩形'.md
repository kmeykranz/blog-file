---
title: 'OpenGL教程 | 1.如何上手OpenGL, 绘制三角形和矩形'
tags:
  - OpenGL
  - C++
  - 图形学
categories: 学习笔记
image: 'https://p.fiveth.cc/img/m/openglcover.webp'
slug: 8ca
date: 2023-05-24 21:34:08
---

## 关于学习OpenGL

OpenGL是学习计算机图形学的必备，虽然现在Vulkan是未来的OpenGL，但目前OpenGL仍是无法被替代的。

Vulkan学习难度过高，个人做项目太耗时间，所以OpenGL依旧是很好的选择。很多大神都认为，先学OpenGL打基础，在学习Vulkan的时候再将原先的知识进行分解转化，是个很好的方法。因为学习Vulkan时，想让屏幕上显示点东西需要做的工作太多了，非常容易放弃。(别说Vulkan了，OpenGL就已经够难了🤕)

所以看了各论坛和思考后，我决定先上手OpenGL。

这是一个非常好的OpenGL教学网站：[英文版](https://learnopengl.com/) | [中文版](https://learnopengl-cn.github.io/)

> 经验：建议打开源代码看着学习，光跟着文档的话思路会很乱

下面是我做的笔记

配合源码食用更佳🤫：[OpenGL学习源码](https://github.com/kevinwu06/LearnOpenGL)

------

## 配置

因为OpenGL不支持，所以我们需要一个库用于显示窗口和处理用户输入 (如GLUT，SDL，SFML和GLFW)

这里我们使用GLFW

### GLFW

一个专门针对OpenGL的C语言库。[GLFW下载](https://www.glfw.org/download.html)

为确保完整性，下载**源代码**后用CMake编译。

### CMake

一个工程文件生成工具。[Cmake下载](https://cmake.org/download/)

### glad

用于简化OpenGL获取函数地址的库。[生成glad](https://glad.dav1d.de/)

### 配置

在vs项目属性中指向include和lib文件夹

将glad/src里的glad.c放入工程文件，并在vs中**添加现有项**

在依赖项里加入

```cpp
glfw3.lib;opengl32.lib
```

## 你好，窗口

### 代码

```cpp
#include <glad/glad.h>
#include <GLFW/glfw3.h>
#include <iostream>

void framebuffer_size_callback(GLFWwindow* window, int width, int height);
void processInput(GLFWwindow* window);

//配置项
const unsigned int SCR_WIDTH = 800;
const unsigned int SCR_HEIGHT = 600;

int main()
{
    //初始化glfw
    glfwInit();
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
    glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);

    // 创建glfw窗口
    GLFWwindow* window = glfwCreateWindow(SCR_WIDTH, SCR_HEIGHT, "LearnOpenGL", NULL, NULL);
    if (window == NULL)
    {
        std::cout << "Failed to create GLFW window" << std::endl;
        glfwTerminate();
        return -1;
    }
    glfwMakeContextCurrent(window);
    //窗口变换
    glfwSetFramebufferSizeCallback(window, framebuffer_size_callback);

    //glad加载opengl指针
    if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress))
    {
        std::cout << "Failed to initialize GLAD" << std::endl;
        return -1;
    }

    //渲染循环
    while (!glfwWindowShouldClose(window))
    {
        //处理输入
        processInput(window);

        //渲染
        glClearColor(0.2f, 0.3f, 0.3f, 1.0f);
        glClear(GL_COLOR_BUFFER_BIT);

        //glfw检查并调用事件，交换缓冲
        glfwSwapBuffers(window);
        glfwPollEvents();
    }

    // glfw清除
    glfwTerminate();
    return 0;
}

//输入处理函数
void processInput(GLFWwindow* window)
{
    if (glfwGetKey(window, GLFW_KEY_ESCAPE) == GLFW_PRESS)
        glfwSetWindowShouldClose(window, true);
}

// glfw，窗口大小变换时自动调用
void framebuffer_size_callback(GLFWwindow* window, int width, int height)
{
    glViewport(0, 0, width, height);
}
```

源代码：[窗口源码](https://github.com/kevinwu06/LearnOpenGL/blob/main/%E7%AA%97%E5%8F%A3.cpp)

## 你好，三角形

现代Opengl渲染至少要设置一个**顶点着色器**和一个**片段着色器**。

### 知识

- VAO (顶点数组对象 Vertex Array Object)
- VBO (顶点缓冲对象 Vertex Buffer Object)
- EBO (元素缓冲对象 Element Buffer Object) 

#### 顶点着色器

Vertex Shader

##### 基础源码

```cpp
#version 330 core
layout (location = 0) in vec3 aPos;

void main()
{
    gl_Position = vec4(aPos.x, aPos.y, aPos.z, 1.0);
}
```

#### 片段着色器

Fragment Shader 计算像素最后的颜色输出

##### 基础源码

```cpp
#version 330 core
out vec4 FragColor;

void main()
{
    FragColor = vec4(1.0f, 0.5f, 0.2f, 1.0f); //输出黄色
} 
```

#### 着色器程序

Shader Program Object

链接多个着色器，将它们合并在一起。

#### 链接顶点属性

指定Opengl如何解释顶点数据。

```cpp
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
glEnableVertexAttribArray(0);
```

- 第一个参数：顶点属性。
- 第二个参数：顶点属性的大小。顶点属性是一个**vec3**，它由3个值组成，所以大小是3。
- 第三个参数：数据的类型，这里是GL_FLOAT(GLSL中**vec***都是由浮点数值组成的)。
- 第四个参数：是否希望数据被标准化(Normalize)。如果我们设置为GL_TRUE，所有数据都会被映射到0（对于有符号型signed数据是-1）到1之间。我们把它设置为GL_FALSE。
- 第五个参数叫做步长(Stride)，它告诉我们在连续的顶点属性组之间的间隔。由于下个组位置数据在3个**float**之后，我们把步长设置为**3 * sizeof(float**)。要注意的是由于我们知道这个数组是紧密排列的（在两个顶点属性之间没有空隙）我们也可以设置为0来让OpenGL决定具体步长是多少（只有当数值是紧密排列时才可用）。（译注: **这个参数的意思简单说就是从这个属性第二次出现的地方到整个数组0位置之间有多少字节**）。
- 最后一个参数：类型是**void***，所以需要我们进行这个奇怪的强制类型转换。它表示位置数据在缓冲中起始位置的偏移量(Offset)。由于位置数据在数组的开头，所以这里是0。我们会在后面详细解释这个参数。

### 代码

#### 硬编码着色器源码

定义全局变量

```cpp
//硬编码着色器源代码
const char* vertexShaderSource = "#version 330 core\n"
"layout (location = 0) in vec3 aPos;\n"
"void main()\n"
"{\n"
"   gl_Position = vec4(aPos.x, aPos.y, aPos.z, 1.0);\n"
"}\0";
const char* fragmentShaderSource = "#version 330 core\n"
"out vec4 FragColor;\n"
"void main()\n"
"{\n"
"   FragColor = vec4(1.0f, 0.5f, 0.2f, 1.0f);\n"
"}\n\0";
```

#### 配置着色器

```cpp
	// 创建&编译 着色器
    // ------------------------------------
    // 顶点着色器vertex shader
    unsigned int vertexShader = glCreateShader(GL_VERTEX_SHADER);
    glShaderSource(vertexShader, 1, &vertexShaderSource, NULL);
    glCompileShader(vertexShader);
    // 检测错误
    int success;
    char infoLog[512];
    glGetShaderiv(vertexShader, GL_COMPILE_STATUS, &success);
    if (!success)
    {
        glGetShaderInfoLog(vertexShader, 512, NULL, infoLog);
        std::cout << "ERROR::SHADER::VERTEX::COMPILATION_FAILED\n" << infoLog << std::endl;
    }
    // 片段着色器fragment shader
    unsigned int fragmentShader = glCreateShader(GL_FRAGMENT_SHADER);
    glShaderSource(fragmentShader, 1, &fragmentShaderSource, NULL);
    glCompileShader(fragmentShader);
    // 检测错误
    glGetShaderiv(fragmentShader, GL_COMPILE_STATUS, &success);
    if (!success)
    {
        glGetShaderInfoLog(fragmentShader, 512, NULL, infoLog);
        std::cout << "ERROR::SHADER::FRAGMENT::COMPILATION_FAILED\n" << infoLog << std::endl;
    }
    // 着色器程序(链接着色器)
    unsigned int shaderProgram = glCreateProgram();
    glAttachShader(shaderProgram, vertexShader);
    glAttachShader(shaderProgram, fragmentShader);
    glLinkProgram(shaderProgram);
    // 检测错误
    glGetProgramiv(shaderProgram, GL_LINK_STATUS, &success);
    if (!success) {
        glGetProgramInfoLog(shaderProgram, 512, NULL, infoLog);
        std::cout << "ERROR::SHADER::PROGRAM::LINKING_FAILED\n" << infoLog << std::endl;
    }
    //删除前面的着色器
    glDeleteShader(vertexShader);
    glDeleteShader(fragmentShader);

    // 链接顶点属性
    // ------------------------------------------------------------------
    float vertices[] = {
        -0.5f, -0.5f, 0.0f, // left  
         0.5f, -0.5f, 0.0f, // right 
         0.0f,  0.5f, 0.0f  // top   
    };
    //VBO顶点缓冲对象，VAO顶点数组对象
    unsigned int VBO, VAO;
    glGenVertexArrays(1, &VAO);
    glGenBuffers(1, &VBO);
    // 先绑定VAO，然后VBO，最后设置顶点属性指针
    //bind the Vertex Array Object first, then bindand set vertex buffer(s), and then configure vertex attributes(s).
    glBindVertexArray(VAO);

    glBindBuffer(GL_ARRAY_BUFFER, VBO);
    glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);

    glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
    glEnableVertexAttribArray(0);

    // 解绑
    glBindBuffer(GL_ARRAY_BUFFER, 0);
    glBindVertexArray(0);
```

**glBufferData参数**:

- GL_STATIC_DRAW ：数据不会或几乎不会改变。
- GL_DYNAMIC_DRAW：数据会被改变很多。
- GL_STREAM_DRAW ：数据每次绘制时都会改变。

#### 绘制三角形

在渲染循环中

```cpp
//画三角形
glUseProgram(shaderProgram);
glBindVertexArray(VAO);
glDrawArrays(GL_TRIANGLES, 0, 3);
```

然后就可以跟三角形说你好了！😎

<img src="https://p.fiveth.cc/img/m/triangle.webp" style="zoom:50%;" />

源代码：[三角形源码](https://github.com/kevinwu06/LearnOpenGL/blob/main/%E4%B8%89%E8%A7%92%E5%BD%A2.cpp)

## Hey，矩形

我们绘制两个三角形来组成一个矩形

```cpp
float vertices[] = {
    // 第一个三角形
    0.5f, 0.5f, 0.0f,   // 右上角
    0.5f, -0.5f, 0.0f,  // 右下角
    -0.5f, 0.5f, 0.0f,  // 左上角
    // 第二个三角形
    0.5f, -0.5f, 0.0f,  // 右下角
    -0.5f, -0.5f, 0.0f, // 左下角
    -0.5f, 0.5f, 0.0f   // 左上角
};
```

但是，我们会看到，有两个顶点是重叠的，这回大大增加开销。所以我们要用到**元素缓冲对象**。

#### 元素缓冲对象

**EBO** 他存储 OpenGL 用来决定要绘制哪些顶点的索引

```cpp
float vertices[] = {
         0.5f,  0.5f, 0.0f,  // top right
         0.5f, -0.5f, 0.0f,  // bottom right
        -0.5f, -0.5f, 0.0f,  // bottom left
        -0.5f,  0.5f, 0.0f   // top left 
    };
    unsigned int indices[] = {  // 从0开始!
        0, 1, 3,  // first Triangle
        1, 2, 3   // second Triangle
    };
```

可以看到现在只用存四个顶点数据

和VAO,VBO一样，创建并绑到缓冲中

```cpp
unsigned int EBO;
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, EBO);
glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(indices), indices, GL_STATIC_DRAW);
```

然后在渲染循环中绘制

```cpp
// 绘制
glUseProgram(shaderProgram);
glBindVertexArray(VAO);
glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0); //绘制元素缓冲中的内容
```

然后我们就得到我们的矩形了

<img src="https://p.fiveth.cc/img/m/rectangle.webp" style="zoom:50%;" />

我们还可以打开线框模式，显示矩形由两个三角形组成

```cpp
glPolygonMode(GL_FRONT_AND_BACK, GL_LINE)
//G_LINE打开，GL_FILL关闭
```

<img src="https://p.fiveth.cc/img/m/rectline.webp" style="zoom:50%;" />

源代码：[矩形源码](https://github.com/kevinwu06/LearnOpenGL/blob/main/%E7%9F%A9%E5%BD%A2.cpp)

------

如果你也到这步了，那么恭喜你🎉，通过了OpenGL最难部分之一。

接下来的路还很长，我们一起加油。
