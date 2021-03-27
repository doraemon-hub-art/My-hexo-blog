---
title: C语言实现飞翔的小鸟小游戏
date: 2021-03-27 15:01:53
tags:
	- -C语言
categories:
	- C语言
cover: /img/di.png
---



参考视频https://www.bilibili.com/video/BV1Xo4y1R7hs

**缺点：撞柱子功能暂未实现**

```c
//飞翔的小鸟
#include<stdio.h>//C语言标准头文件
#include<graphics.h>//图形库头文件
#include<conio.h>//按键处理
#include<time.h>//随机函数
#include<mmstream.h>//多媒体库
#pragma comment(lib,"winmm.lib")
/********************************************
					数据设计
*********************************************/
IMAGE background;
IMAGE bigBird[2]; //bigBird[0] bigBird[1]
IMAGE endImg[2];
IMAGE up[2];
IMAGE down[2];
HWND hwnd;//句柄-表示的是窗口的意思
//结构体
struct bird
{
	int x;//鸟的x和y坐标
	int y;
	int speed; //鸟的速度
};
//鸟的属性
struct bird myBird = { 124,304,100 };
//加载资源：把图片和变量名绑定在一起
void loadResource()
{
	//先加载掩码如 再加载背景图
	loadimage(&background, "./images/background.bmp");
	loadimage(&bigBird[0], "./images/birdy.bmp",48,48);
	loadimage(&bigBird[1], "./images/bird.bmp",48,48);
	loadimage(&endImg[0], "./images/endy.bmp");
	loadimage(&endImg[1], "./images/end.bmp");

	loadimage(&down[0], "./images/downy.bmp");
	loadimage(&down[1], "./images/down.bmp");

	loadimage(&up[0], "./images/upy.bmp");
	loadimage(&up[1], "./images/up.bmp");
}
/********************************************
					鸟的模块
					1.绘制鸟的过程
					2.按键操作控制鸟的过程
					3.音乐部分---多线程知识
					要开辟一个线程来播放音乐，要不他会影响背景的效果
*********************************************/
//绘制鸟的过程
void drawBigbird(int x ,int y)
{
	//贴图（掩码图）
	putimage(x, y, &bigBird[0], SRCAND);
	putimage(x, y, &bigBird[1], SRCPAINT);
}
//线程处理函数---》C语言中函数指针
DWORD WINAPI playMusic(LPVOID pVoid)
{
	mciSendString("open jump.mp3", 0, 0, 0);
	mciSendString("play jump.mp3 wait", 0, 0, 0);
	mciSendString("clos jump.mp3", 0, 0, 0);
	return 0;
}
/********************************************
				柱子部分
				1.画柱子
				2.初始化柱子
				3.移动柱子
*********************************************/
struct pillar
{	
	//上面柱子的属性
	int x ;
	int y ;
	int h ;
	//根据上面柱子的属性能够退出下面柱子的属性
	//Height - h
};
struct pillar zhuzi[3];
//初始化柱子
void initPillar(struct pillar zhuzi[], int i)
{
	zhuzi[i].h = rand() % 100 + 160;
	zhuzi[i].y = 0;
	zhuzi[i].x = 288; 
}
//画柱子
void drawPillar(struct pillar zhuzi)
{
	//上面的柱子
	putimage(zhuzi.x, 0, 52, zhuzi.h,&down[0],0,320 - zhuzi.h,SRCAND);
	putimage(zhuzi.x, 0, 52, zhuzi.h, &down[1], 0, 320 - zhuzi.h, SRCPAINT);
	//下面的柱子
	putimage(zhuzi.x, 512-(320-zhuzi.h), 52, 320-zhuzi.h, &up[0], 0,0, SRCAND);
	putimage(zhuzi.x, 512 - (320 - zhuzi.h), 52, 320 - zhuzi.h, &up[1], 0, 0, SRCPAINT);

}
/********************************************
			通用性技术：
			1.并发编程
			2.网络编程
			3.数据库编程
*********************************************/
//按键交互
void keyDown()
{
	char userKey = '\0';
	userKey = _getch();
	//暂停功能
	if (userKey == ' ')
	{
		while (_getch() != ' ');
	}
	switch (userKey)
	{
	case 'w':
	case 'W':
	case 72:
		myBird.y -= myBird.speed;
		CreateThread(NULL,NULL, playMusic, NULL, NULL, NULL);
		break;
	default:
		break;
	}
}
//碰地板和上边界处理
int hitFloor()
{
	if (myBird.y <= 0 || myBird.y >= (512 - 48))
	{
		return 1;
	}
	return 0;
}
//结束动画
void gameOverAction()
{
	int  x = 60;
	int y = 608;
	while (y >= 240)
	{
		putimage(0, 0, &background);
		putimage(x, y, &endImg[0], SRCAND);
		putimage(x, y, &endImg[1], SRCPAINT);
		y -= 50;
		Sleep(50);
	}
	MessageBox(hwnd,"GameOver You Die!","提示",MB_OK);
}
/********************************************
					入口函数
*********************************************/
int main(void)
{	
	srand((unsigned int)time(NULL));
	//加载资源
	loadResource();
	//创建图形窗口
	initgraph(288, 608);
	//柱子
	for (int i = 0; i < 3; i++)
	{
		initPillar(zhuzi, i);
		zhuzi[i].x = 288 + i * 150;
	}
	while (1)
	{
		putimage(0, 0, &background);
		//加载图片
		drawBigbird(myBird.x, myBird.y);
		for (int i = 0; i < 3; i++)
		{
			zhuzi[i].x -= 10;
			if(zhuzi[i].x < (-52 - 150))
			{
				initPillar(zhuzi, i);
			}
		}
		for (int i = 0; i < 3; i++)
		{
			drawPillar(zhuzi[i]);
		}
		if (hitFloor())
		{
			gameOverAction();
			break;
		}
		//鸟的自由落体
		myBird.y += 10;
		//只有按键处理，没有按键不处理
		if (_kbhit())
		{
			keyDown();
		}
		//延时处理
		Sleep(50);
	}
	_getch();//等待用户按键-防止闪屏
	//关闭窗口
	closegraph();
	return 0;
}
```

