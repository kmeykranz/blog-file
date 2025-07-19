---
title: Hexo博客优化：批量修改md文件名
toc: true
tags:
  - 博客
  - Hexo
  - Python
categories: 博客相关
image: 'https://p.fiveth.cc/img/m/hexo.jpg'
slug: fbe8
date: 2024-04-09
---

今天整理博客，发现我给文章取的文件名比较混乱，要找文章的时候会很不方便。



一般我们都是用`hexo new file_name`创建文章的md文件的，然后文章的标题是在md文件的信息栏定义的。而我们很多时候是开始写文章才想好标题，随着写作标题也会发生变动，这导致最后我们文章标题和文件名是不同的，加大了我们的管理难度。



所以我们会在改动文章标题的时候，将文件名也改成一样的。但是这样手动改非常麻烦。因此我配合ChatGPT用Python写了个一键批量修改文件名的小脚本。这样我们就可以很方便的一键将文件名全部都同步成文章的标题。

![image1](https://p.fiveth.cc/img/2024/04/09144430.webp)

## 使用方法

### 方法1. exe文件

脚本已经打包成exe文件，大家可以直接下载使用，下载后放到`_posts`文件夹下运行即可。

https://wwf.lanzoum.com/icY4q1ufi7na 密码:ff6u

> 注意：如果标题中有不支持的特殊符号，会导致运行失败。可以往下看，自行用python调整源代码后运行

记得运行完后将脚本移出文件夹，防止被上传。

### 方法2. python运行

如果有调试需求，大家也可以自己用python编译使用。

**下面是方法**：

- 首先要安装[python](https://www.python.org/)。

- 在`_posts`文件夹中创建一个`rename.txt`文件，复制粘贴下列代码进去：

```python
import os
dir_path = "./"
i = 0
for file in os.listdir(dir_path):
    ## Skip '.' and '..'
    if file == "." or file == "..":
        continue
    ## Construct the full path to the file
    file_path = os.path.join(dir_path, file)
    ## Read the file
    try:
        with open(file_path, 'r', encoding='utf-8') as f:
            lines = f.readlines()
    except UnicodeDecodeError:
        print(f"Skipping file due to encoding issue: {file}")
        continue
    ## Skip files with less than 2 lines
    if len(lines) < 2:
        continue
    ## Extract and process the title from the second line
    title = lines[1].strip().replace('title:', '').strip()
    ## Remove any marks from the title
    title = title.replace('?', '')
    title = title.replace('|', '-')
    ## Rename
    new_filename = title + '.md'
    new_file_path = os.path.join(dir_path, new_filename)
    os.rename(file_path, new_file_path)
    i += 1
print(i)
```

- 然后将文件名后缀修改成`.py`

- 在文件夹空白处右键，点击`在终端中打开`

- 在终端中输入`python rename.py`即可运行脚本

------

另外，去年我一直是随缘更博客，接下来我会开始有规律性的定期发文章，分享我的生活和学习到的知识。做博客的精髓在于长期坚持，一直有新内容才会有人关注。一起加油吧😊
