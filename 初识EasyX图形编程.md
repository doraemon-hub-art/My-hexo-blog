---
title: 初识EasyX图形编程
date: 2021-05-09 13:01:35
tags:
  - -EasyX
  - -图形编程
  - -C语言
categories:
  - 图形编程
cover: /img/EasyX.png
---

***

相关视频——[【C/C++/EasyX】学编程，做游戏，小白快速入门图形编程，零基础入门到精通，学习就是这么快乐_哔哩哔哩 (゜-゜)つロ 干杯~-bilibili](https://www.bilibili.com/video/BV11p4y1i74A?p=1)

***

# 1.基本说明

- EasyX是针对C++的图形库，可以帮助C/C++初学者快速上手图形和游戏编程。
- 比如 ,可以基于EasyX图形库很快用几何图形画一个房子，或者一辆移动的小车，可以编写俄罗斯方块 、贪吃蛇、黑白棋等小游戏。
- 许多人学编程是从C语言入门的，而目前的现状是“
  - 学校值只教基础语法，一直在黑窗口练习，同学们学的很乏味。、
  - 即使有的学校教图形编程，也是使用一些难度较高的， 比如Win32,OpenlGl门槛依然很高，初学者容易收到打击。
  - 开始引出我们的EasyX。

# 2.原理

​		基于Windows图形编程，将Windows下的复杂程序过程进行封装,将Windows下的编程过程隐藏，给用户提供一个简单熟悉的接口。用户对于图形库中函数的调用，最终都会由Windows的底层API实现。

# 3.安装

- Easyx图形库支持Vs各种版本，下载解压后，直接执行安装程序即可。
- 头文件graphics.h
- 帮助文档[EasyX 文档 - 基本说明](https://docs.easyx.cn/zh-cn/intro)
- 下载[EasyX Graphics Library for C++](https://easyx.cn/)

# 4.颜色

​	用RGB宏合成颜色，实际上合成出来的颜色是一个十六进制的的整数。

​	**每个颜色部分的值都是0~255**

# 5.坐标和设备

- 坐标默认的原点在窗口的左上角，X轴向右为正，Y 轴向下为正，度量单位是像素点。
- 设备：简单来说，就是绘图表面。
  - 在EasyX中,设备分两种，一种是默认的绘图窗口另一种是IMAGE对象。通过SetWorkinglmage()函数可以设置当前用于绘图的设备。设置当前用于绘图的设备后,所有的绘图函数都会绘制在该设备上。(后面再去理解)

# 6.窗口函数

​	窗口函数用于窗口的一些操作

```C
initgraph(int width,int height,int flag = NULL);//用于初始化绘图窗口
//width 指定窗口的宽度
//height 指定窗口的高度
//flag 窗口的样式默认为NULL
```

```C
closegraph();//关闭绘图窗口
```

```C
cleardevice();//清空绘图设备
```

# 7.图形绘制函数

- 图形绘制函数用于在窗口上绘制各种图形。

- 绘图函数从填充样式分类可以分为无填充，有边框填充，无边框三种。

``` C
以画圆为例
    circle()无填充
    fillcircle()有边框填充
    solidcircle()无边框填充
```

区别：

![image-20210508202623558](/images/初识EasyX图形编程.assets/image-20210508202623558.png)

- 从形状来分，常用的可以分为八种。

![image-20210508201228945](/images/初识EasyX图形编程.assets/image-20210508201228945.png)



- 设置填充颜色setfillcolor()；
- 设置线条颜色setlinecolor();
- 设置线条样式setlinestyle();高，宽，字体

# 8.文字绘制函数

- 文字绘制函数用于在窗口上绘制文字

![image-20210508202106822](/images/初识EasyX图形编程.assets/image-20210508202106822.png)

#   9.图像处理函数

- 图像处理函数用于在窗口上显示图片

![image-20210508202202224](/images/初识EasyX图形编程.assets/image-20210508202202224.png)





# 10.鼠标消息函数

- 鼠标消息函数用于获取鼠标的信息

![image-20210509115406258](/images/初识EasyX图形编程.assets/image-20210509115406258.png)

# 11.键盘消息函数

- 键盘消息函数用于获取键盘按键消息

![image-20210509121713266](/images/初识EasyX图形编程.assets/image-20210509121713266.png)

# 12.其他函数

![image-20210509122340561](/images/初识EasyX图形编程.assets/image-20210509122340561.png)

# 13.音乐播放

![image-20210509123501332](/images/初识EasyX图形编程.assets/image-20210509123501332.png)

# 易错集锦

1. 源文件问题： fata1 error c1189: #error : EasyXis only for C++。

   **后缀要是cpp**

2. 参数错误，找不到对应的函数：error C2665： "outtextxy":2个重载中没有一个可以转换所有参数类型。

   是由于字符集导致的，1.在字符串前面加上大写的L，2.用TEXT(_T())把字符串包起起来。

   不需要添加任何代码，项目-属性-常规-字符集-使用多字节字符集


