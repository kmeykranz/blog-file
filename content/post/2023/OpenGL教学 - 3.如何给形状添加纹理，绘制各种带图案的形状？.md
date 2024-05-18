---
title: OpenGL教学 | 3.如何给形状添加纹理，绘制各种带图案的形状？
tags:
  - OpenGL
  - C++
  - 图形学
categories: 学习笔记
image: 'https://p.fiveth.cc/img/m/grasscover.jpg'
slug: 7cf1
date: 2023-05-30 18:33:42
---

本篇将学习OpenGL中关于纹理的各种知识。纹理就是图片，用于给模型添加细节。

# 纹理环绕方式

如果我们把纹理坐标设置在范围之外会发生什么？OpenGL默认的行为是重复这个纹理图像。下面更多的选择：

| 环绕方式           | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| GL_REPEAT          | 对纹理的默认行为。重复纹理图像。                             |
| GL_MIRRORED_REPEAT | 和GL_REPEAT一样，但每次重复图片是镜像放置的。                |
| GL_CLAMP_TO_EDGE   | 纹理坐标会被约束在0到1之间，超出的部分会重复纹理坐标的边缘，产生一种边缘被拉伸的效果。 |
| GL_CLAMP_TO_BORDER | 超出的坐标为用户指定的边缘颜色。                             |

设置坐标轴环绕方式（s、t（如果是使用3D纹理那么还有一个r））

```cpp
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_MIRRORED_REPEAT);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_MIRRORED_REPEAT);
```

如果选择GL_CLAMP_TO_BORDER选项，需要指定一个边缘的颜色。

```cpp
float borderColor[] = { 1.0f, 1.0f, 0.0f, 1.0f };
glTexParameterfv(GL_TEXTURE_2D, GL_TEXTURE_BORDER_COLOR, borderColor);
```

# 纹理过滤

如何将**纹理像素**(Texture Pixel)映射到纹理坐标，让分辨率清晰

纹理过滤有很多个选项，但是现在我们只讨论最重要的两种：

### GL_NEAREST

邻近过滤 (Nearest Neighbor Filtering):

选择中心点最接近纹理坐标的那个像素

![img](https://learnopengl-cn.github.io/img/01/06/filter_nearest.png)

### GL_LINEAR

线性过滤 (linear Filtering):

基于纹理坐标附近的纹理像素，计算出一个插值 (心距离纹理坐标越近,贡献越大)

![img](https://learnopengl-cn.github.io/img/01/06/filter_linear.png)

### 效果对比

![img](https://learnopengl-cn.github.io/img/01/06/texture_filtering.png)

我们可以在纹理被缩小的时候使用邻近过滤，被放大时使用线性过滤。方法：

```cpp
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_NEAREST);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
```

# 多级渐远纹理

(Mipmap)：一系列的纹理图像，后一个纹理图像是前一个的二分之一。

功能：提升真实性、提升性能。

![img](https://learnopengl-cn.github.io/img/01/06/mipmaps.png)

| 过滤方式                  | 描述                                                         |
| ------------------------- | ------------------------------------------------------------ |
| GL_NEAREST_MIPMAP_NEAREST | 使用最邻近的多级渐远纹理来匹配像素大小，并使用邻近插值进行纹理采样 |
| GL_LINEAR_MIPMAP_NEAREST  | 使用最邻近的多级渐远纹理级别，并使用线性插值进行采样         |
| GL_NEAREST_MIPMAP_LINEAR  | 在两个最匹配像素大小的多级渐远纹理之间进行线性插值，使用邻近插值进行采样 |
| GL_LINEAR_MIPMAP_LINEAR   | 在两个邻近的多级渐远纹理之间使用线性插值，并使用线性插值进行采样 |

设置方法：

```cpp
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR_MIPMAP_LINEAR);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
```

# 加载与创建纹理

将图像加载到应用中，需要自己写一个图像加载器，或者使用支持多种格式的图像加载库。

## stb_image.h

[stb_image.h下载链接](https://github.com/nothings/stb/blob/master/stb_image.h)

将头文件加入你的工程，并在源文件中输入以下代码引入：

```cpp
#define STB_IMAGE_IMPLEMENTATION //让头文件只包含相关的函数定义源码
#include "stb_image.h"
```

接下来，我们用stb_image加载图片

```cpp
int width, height, nrChannels;
unsigned char *data = stbi_load("container.jpg", &width, &height, &nrChannels, 0);
```

加载图片前我们为了防止图片颠倒输出，我们可以输入

```cpp
stbi_set_flip_vertically_on_load(true); //OpenGL会自动将图片颠倒，所以我们要上下翻转
```

# 生成纹理

创建纹理对象

```cpp
unsigned int texture;
glGenTextures(1, &texture);
```

生成纹理

```cpp
glTexImage2D(GL_TEXTURE_2D, 0, GL_RGB, width, height, 0, GL_RGB, GL_UNSIGNED_BYTE, data);
glGenerateMipmap(GL_TEXTURE_2D);
```

- 第一个参数指定了纹理目标(Target)。设置为GL_TEXTURE_2D意味着会生成与当前绑定的纹理对象在同一个目标上的纹理（任何绑定到GL_TEXTURE_1D和GL_TEXTURE_3D的纹理不会受到影响）。
- 第二个参数为纹理指定多级渐远纹理的级别，如果你希望单独手动设置每个多级渐远纹理的级别的话。这里我们填0，也就是基本级别。
- 第三个参数：纹理储存格式。(RGB\RGBA)
- 第四个和第五个参数设置最终的纹理的宽度和高度。
- 下个参数应该总是被设为`0`（历史遗留的问题）。
- 第七第八个参数定义了源图的格式和数据类型。我们使用RGB值加载这个图像，并把它们储存为`char`(byte)数组，我们将会传入对应值。
- 最后一个参数：图像数据。

最后还要释放图像内存

```cpp
stbi_image_free(data);
```

**生成纹理的整个过程**：

```cpp
unsigned int texture;
glGenTextures(1, &texture);
glBindTexture(GL_TEXTURE_2D, texture);
// 为当前绑定的纹理对象设置环绕、过滤方式
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_REPEAT);   
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_REPEAT);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
// 加载并生成纹理
int width, height, nrChannels;
stbi_set_flip_vertically_on_load(true);//OpenGL会自动将图片颠倒，所以我们要上下翻转
unsigned char *data = stbi_load("container.jpg", &width, &height, &nrChannels, 0);
if (data)
{
    glTexImage2D(GL_TEXTURE_2D, 0, GL_RGB, width, height, 0, GL_RGB, GL_UNSIGNED_BYTE, data);
    glGenerateMipmap(GL_TEXTURE_2D);
}
else
{
    std::cout << "Failed to load texture" << std::endl;
}
stbi_image_free(data);
```

# 应用纹理

接下来我们来应用纹理，绘制一个带我的世界草块纹理的矩形：[草块图片](https://github.com/kevinwu06/LearnOpenGL/blob/main/grass.jpg)

```cpp
float vertices[] = {
//     ---- 位置 ----       ---- 颜色 ----     - 纹理坐标 -
	0.5f,  0.5f, 0.0f,   1.0f, 0.0f, 0.0f,   1.0f, 1.0f,   // 右上
    0.5f, -0.5f, 0.0f,   0.0f, 1.0f, 0.0f,   1.0f, 0.0f,   // 右下
    -0.5f, -0.5f, 0.0f,   0.0f, 0.0f, 1.0f,   0.0f, 0.0f,   // 左下
    -0.5f,  0.5f, 0.0f,   1.0f, 1.0f, 0.0f,   0.0f, 1.0f    // 左上
};
unsigned int indices[] = {
    0, 1, 3, // first triangle
    1, 2, 3  // second triangle
};
```

在循环中绘制

```cpp
glBindTexture(GL_TEXTURE_2D, texture);
glBindVertexArray(VAO);
glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0);
```

我们的草块就出来了！

我们再在草块上加上顶点颜色：修改片段着色器

<img src="https://p.fiveth.cc/img/m/grasswin.webp" style="zoom:50%;" />

源代码：[纹理应用源码](https://github.com/kevinwu06/LearnOpenGL/blob/main/%E7%BA%B9%E7%90%86%E5%BA%94%E7%94%A8.cpp)

```cpp
FragColor = texture(ourTexture, TexCoord) * vec4(ourColor, 1.0);
```

<img src="https://p.fiveth.cc/img/m/colorgrass.webp" style="zoom:50%;" />

# 纹理单元

纹理单元(Texture Unit)：纹理的位置值

绑定前激活纹理单元：

```cpp
glActiveTexture(GL_TEXTURE0); // 在绑定纹理之前先激活纹理单元
glBindTexture(GL_TEXTURE_2D, texture);
```

**我们来做个笑脸草块：**

先修改一下片段着色器：

```cpp
uniform sampler2D texture1;
uniform sampler2D texture2;

void main()
{
    FragColor = mix(texture(texture1, TexCoord), texture(texture2, TexCoord), 0.2);
    //mix输出两个texture的混合值
}
```

再设置纹理单元

```cpp
ourShader.use();
glUniform1i(glGetUniformLocation(ourShader.ID, "texture1"), 0); //手动设置
ourShader.setInt("texture2", 1);
```

现在我们绑定多个纹理并绘制：

```cpp
glActiveTexture(GL_TEXTURE0);
glBindTexture(GL_TEXTURE_2D, texture1);
glActiveTexture(GL_TEXTURE1);
glBindTexture(GL_TEXTURE_2D, texture2);

glBindVertexArray(VAO);
glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0);
```

就可以看到我们的笑脸草块了

<img src="https://p.fiveth.cc/img/m/smilegrass.webp" style="zoom:50%;" />

源代码：[混合纹理源码](https://github.com/kevinwu06/LearnOpenGL/blob/main/%E6%B7%B7%E5%90%88%E7%BA%B9%E7%90%86.cpp)
