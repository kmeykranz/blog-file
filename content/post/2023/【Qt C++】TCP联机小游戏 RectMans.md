---
title: 【Qt C++】TCP联机小游戏 RectMans
tags:
  - 游戏
  - C++
  - Qt
  - TCP
categories: 我的项目
image: 'https://p.fiveth.cc/img/m/rectmans.jpg'
slug: '1841'
date: 2023-05-12 00:00:00
---

学习TCP后用基于Qt TCP库做了一个联机小游戏🤼

断断续续大概写了一周，程序源码放在文章底部了。

# 结构思路

游戏通过服务端或者客户端进入，只支持两玩家。

服务端进入：自己显示红色衣服，对方蓝色

客户端进入：自己显示蓝色衣服，对方红色

<img src="https://p.fiveth.cc/img/m/rectman.webp" style="zoom:30%;" />

# 代码

## 位置传输

### 发送位置

将int[]转成QByteArray达到发送多数据的效果

```cpp
void Server::sendData(int x_self,int y_self,int dir_self)
{
    if(isConnected){
        //int[] 转 QByteArray
        int  self[3] = {x_self,y_self,dir_self};
        QByteArray array;
        array.append((char*)self, sizeof(int) * 3);
        socket.write(array);
    }
}
```

### 接收位置

将QByteArray转成int[]

```cpp
void Server::receiveData()
{
    // QByteArray 转 int[]
    QByteArray array=socket.readAll();
    int data[3];
    for (int i=0; i<3; i++)
    {
        int unTemp;
        memcpy(&unTemp, array.data() + sizeof(int) * i, sizeof(int));
        data[i] = unTemp;
    }
    x=data[0];
    y=data[1];
    dir=data[2];
}
```

## 相对位置

自己始终显示在屏幕中间，只显示方向的改变。

移动时改变坐标但不显示。

游戏中的物体显示在 **[屏幕中心坐标+物体坐标-自己坐标]**

物体包括：对方玩家，地图，树木

```cpp
//刷新函数，放在游戏主循环中
void Map::draw(int a,int b) //传入自己坐标
{
    map->move(CENTER_X+x-a,CENTER_Y+y-b); //移动到相对位置
}
```

## 树木伪3D显示

很简单，当玩家y坐标大于树时显示在上，小于时显示在下。

这段代码是有bug的，有两名玩家时，树木上下就会出现问题。

主要是我没有找到将Label下移或者上移一层的函数，只有置顶和置底，所以代码逻辑很麻烦🤡。

```cpp
if((tree[i]->getY()<player1->getY()+70&&tree[i]->getY()>player1->getY()-45)
||(tree[i]->getY()<player2->getY()+70&&tree[i]->getY()>player2->getY()-45)){
        tree[i]->Raise();
        if(tree[i]->getY()<player1->getY()-45){
            player1->Raise();
        }
        if(tree[i]->getY()<player2->getY()-45){
            player2->Raise();
    	}
    }
else{
    tree[i]->Lower();
    }
```

### 树木和敌人随机位置生成

```cpp
for(int i=1;i<TREE_NUM;i++){
    	//用QRandomGenerator库，随机生成-1400到1400之间的数字
        tree[i]=new Object(QRandomGenerator::global()->bounded(-1400,1400),
                           QRandomGenerator::global()->bounded(-1400,1400),1,this);
}
```

### 僵尸追踪

僵尸首先会判断与两个玩家间的距离，追踪距离更近的玩家。

```cpp
void Enemy::chase(Player *p1, Player *p2)
{
    int Xp1=p1->getX(),Yp1=p1->getY(),Xp2=p2->getX(),Yp2=p2->getY();
    int disP1=(x-Xp1)*(x-Xp1)+(y-Yp1)*(y-Yp1);
    int disP2=(x-Xp2)*(x-Xp2)+(y-Yp2)*(y-Yp2);
    if(disP1<disP2){
        if(x<Xp1){
            x+=speed;
            enemy->setPixmap(enemy_r);
        }
        else if(x>Xp1){
            x-=speed;
            enemy->setPixmap(enemy_l);
        }
        if(y<Yp1){
            y+=speed;
        }
        else if(y>Yp1){
            y-=speed;
        }
    }
    else{
        if(x<Xp2){
            x+=speed;
            enemy->setPixmap(enemy_r);
        }
        else if(x>Xp2){
            x-=speed;
            enemy->setPixmap(enemy_l);
        }
        if(y<Yp2){
            y+=speed;
        }
        else if(y>Yp2){
            y-=speed;
        }
    }
}
```

------

# 视频演示

<iframe src="//player.bilibili.com/player.html?aid=443100831&bvid=BV1oL411h7Ms&cid=1112953964&page=1&autoplay=0" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

# 源码

源码：https://github.com/kevinwu06/RectMans
