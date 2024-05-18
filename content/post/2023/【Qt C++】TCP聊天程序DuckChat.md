---
title: 【Qt C++】TCP聊天程序DuckChat
tags:
  - C++
  - Qt
  - TCP
categories: 我的项目
image: 'https://p.fiveth.cc/img/m/duckchat.jpg'
slug: dcd3
date: 2023-04-25 00:00:00
---

用Qt和C++开发了一个基于TCP协议的通信程序，源码放在文章底部了。

TCP是一种传输协议，以服务器和客户端的形式运行。

Qt中提供了QTcpServer类来编写服务器端程序，以及QTcpSocket类来编写客户端程序。

服务端开启对一个端口的监听，等待客户端连入。

<img src="https://p.fiveth.cc/img/m/duckchat.webp" style="zoom:30%;" />

# 服务端

服务端继承于QTcpServer类，加上Q_OBJECT

```c++
//头文件
class Server : public QTcpServer
{
    Q_OBJECT
}
//源文件
Server::Server(QObject *parent)
    : QTcpServer{parent}
{
}
```

## 查询ip和端口

```
netstat -ano 活动连接列表

ipconfig /all 查看ip
```

## 监听

服务端用QTcpServer监听端口，用QTcpSocket 读取和写入

```C++
server->listen(QHostAddress::Any,8888); //开始监听端口
```

## 连入

连接后自动触发的函数，开始接受数据

```c++
void Server::incomingConnection(qintptr handle){
    socket.setSocketDescriptor(handle);
    connect(&socket,SIGNAL(readyRead()),this,SLOT(receiveData()));
}
```

## 读取

```c++
void Server::receiveData()
{
    QByteArray message=socket.readAll();
    str=message.data(); //转为QString
}
```

## 发送

```c++
void Server::sendData(QString text)
{
    socket.write(text.toUtf8());
}
```



# 客户端

用QTcpSocket实现连接及收发

```c++
QTcpSocket* socket;
socket.connectToHost(IP,PORT); //连接
if(socket.waitForConnected(1000)){  //等待1秒连接
        qDebug("成功连接聊天室...\n");
    }
    else{
        qDebug("连接失败...\n");
        return;
    }
connect(&socket,SIGNAL(readyRead()),this,SLOT(receiveData()));
```

## 接受和发送

接受和发送与服务端相同

# 界面UI

QSS是QT受CSS启发做的样式表语言，在简单的qt程序制作中非常好用。

## 界面背景

在坐标(0,0)处放QLabel并拉满整个窗口，样式表中放入照片，照片要先导入到qrc资源文件中

```css
QLabel
{
	border-image: url(:/Images/duckl.png);
}
```

## 按钮样式表

hover是鼠标放在上面显示的样式

```css
  QPushButton {
      border: 0.5px solid white;
      border-radius: 6px;
      background-color: rgb(90,194,198);
      min-width: 40px;
	  font-family: "Microsoft YaHei";
	  font-size:11pt;
	  font-weight: bold;
	  color:white;
  }
 QPushButton:hover { 
	border: 0.5px solid white;
      border-radius: 6px;
      background-color: #1fab89;
      min-width: 40px;
	  font-family: "Microsoft YaHei";
	  font-size:10pt;
	  font-weight: bold;
	  color:white;
 }
```

## 全局样式表

编辑基类widget样式表

```css
QWidget{
border: none;
}
QLineEdit {
      border: 0.5px solid rgb(147, 150, 154);
      border-radius: 6px;
      background-color: rgba(40, 44, 52,150);
      min-width: 80px;
	  font-family: "Microsoft YaHei";
	  font-size:11pt;
	  font-weight: bold;
	  color:rgb(147, 150, 154);
}
```

# 程序源码

源码：https://github.com/kevinwu06/DuckChat
