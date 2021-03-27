---
title: C语言贪吃蛇小程序
date: 2021-03-25 20:37:25
tags:
	- -C语言
categories:
	- C语言
cover: /img/jianpan2.png
---

参考视频

https://www.bilibili.com/video/BV1LN41197zV?from=search&seid=15462998985727977257

**代码有点缺陷：1.食物有可能会生成在吃不到的地方**

​						**2.吃掉食物的音效添加失败**

```c
//涉及、 结构体 、循环、 函数 、easyx-是一个图形库帮助做界面的、数组、枚举
//1做界面 创建一个窗口 图形窗口
//2创建一个蛇 蛇的结构 
#include<stdio.h>
#include<graphics.h>
#include<conio.h>
#include<stdlib.h >
//多媒体设备接口的两个东西
#include<mmsystem.h>
#pragma comment(lib,"winmm.lib")
#define SNAKE_NUM 500 //蛇的最大节数
enum DIR
{
	UP,
	DOWN,
	LEFT,
	RIGHT,

};
//蛇的结构
struct Snake
{
	int size;//蛇的节数
	int dir;//蛇的方向
	int speed;//蛇的速度
	POINT coor[SNAKE_NUM];//坐标
}snake;
//食物的结构
struct Food
{
	int x;
	int y;
	int r;//食物的半径(大小）
	bool flag;//食物是否被吃了的标记
	DWORD color;//食物的颜色
}food;
//数据的初始化
void GameInit()
{
	//播放背景音乐
	mciSendString("open ./res/snake_bgm.mp3 alias  BGM", 0, 0, 0);
	mciSendString("play BGM repeat", 0, 0, 0);
	//init 初始化 graph 图形窗口	SHOWCONSOLE-显示控制台
	initgraph(600, 480);
	//设置随机数种子
	//GetTickCount获取系统从开机到现在所经过的毫秒数
	srand(GetTickCount());
	//初始化 蛇 一开始有3节
	snake.size = 3;
	snake.speed = 10;
	snake.dir = RIGHT;//初始化方向
	for (int i = 0; i < snake.size; i++ )
	{
		//横着的是x轴,像右为正方向
		//竖着的是y轴,向下为正方向
		snake.coor[i].x = 40-10*i; 
		snake.coor[i].y = 10;
	}
	//初始化食物
	//rand-随机函数-随机生成一个整数，但是如果没有设置随机数种子，每次产生的都是固定的整数。
	//设置种子需要头文件 stdlib.h
	//一般把时间作为随机数种子，因为时间在不断变化的。
	food.x = rand() % 640;
	food.y = rand() % 480;
	food.color = RGB(rand() % 256, rand() % 256, rand() % 256);
	food.r = rand() % 10+5;
	food.flag = true;
}
//
void GameDraw()
{
	//双缓冲绘图 -防止卡顿
	BeginBatchDraw();
	//设置背景颜色-两步
	setbkcolor(RGB(28, 115, 119));
	cleardevice();//清除图形屏幕
	//绘制蛇
	setfillcolor(RED); 
	for (int i = 0; i < snake.size; i++)
	{
		solidcircle(snake.coor[i].x, snake.coor[i].y, 5);//此函数用来画填充圆
	}
	//绘制食物
	//判断食物是否存在
	if (food.flag)
	{
		solidcircle(food.x, food.y,food.r);
	}
	//双缓冲结束
	EndBatchDraw();
}
//蛇的移动
void SnakeMove()
{
	//移动是什么发生改变？  ---坐标
	//**
	//让身体跟着头移动
	for (int i = snake.size - 1; i >0 ; i--)
	{		
		snake.coor[i] = snake.coor[i - 1];
	}
	//判断方向
	switch (snake.dir)
	{
	case UP:
		snake.coor[0].y-=snake.speed;
		if (snake.coor[0].y <= 0)//超出了上边界
		{
			snake.coor[0].y = 480;
		}
		break;
	case DOWN:
		snake.coor[0].y+= snake.speed;
		if (snake.coor[0].y>= 480)//超出了下边界
		{
			snake.coor[0].y = 0;
		}
		break;
	case LEFT:
		snake.coor[0].x-= snake.speed;
		if (snake.coor[0].x <= 0)//超出了左边界
		{
			snake.coor[0].x = 600;
		}
		break;
	case RIGHT:
		snake.coor[0].x+= snake.speed;
		if (snake.coor[0].x >= 600)//超出了右边界
		{
			snake.coor[0].x = 0;
		}
		break;
	}
}
//通过按键改变蛇的移动方向
void keycontrol()
{	
	//判断一下有没有按键
	if (_kbhit())//如果有按键就返回1真
	{
		//读取键盘输入
		switch (_getch())//_getch是个阻塞函数，
		{
			//判断输入的是什么
			//键值 72 80 75 77 上下左右
		case 'w':
		case 'W':
		case 72:
			if (snake.dir != DOWN)
			{
				snake.dir = UP;
			}
			break;
		case 's':
		case 'S':
		case 80:
			if (snake.dir != UP)
			{
				snake.dir = DOWN;
			}
			break;
		case 'a':
		case 'A':
		case 75:
			if (snake.dir != RIGHT)
			{
				snake.dir = LEFT;
			}
			break;
		case 'd':
		case 'D':
		case 77:
			if (snake.dir != LEFT)
			{
				snake.dir = RIGHT;
			}
			break;
			//空格暂停
		case ' ':
			while (1)
			{
				if (_getch() == ' ')
					return;
			}
			break;
		}
	}
}
//判断蛇吃食物
void EatFood()
{
	if (food.flag && snake.coor[0].x >= food.x-food.r && snake.coor[0].x <= food.x+food.r &&
		snake.coor[0].y >= food.y - food.r && snake.coor[0].y <= food.y + food.r)
	{
		food.flag = false;
		snake.size++;
		//下面的吃掉音效添加失败
		mciSendString("open ./res/eatfood.mp3 alias  BGM2", 0, 0, 0);
		mciSendString("play BGM2 ", 0, 0, 0);
	}
	//食物被吃掉之后再次初始化
	if (!food.flag)
	{	
		food.x = rand() % 640;
		food.y = rand() % 480;
		food.color = RGB(rand() % 256, rand() % 256, rand() % 256);
		food.r = rand() % 10 + 5;
		food.flag = true;
	}
	//可以加一个分数，吃一个食物加n分
}

int main(void)
{	
	GameInit();
	


	while (1) //while 1直接卡死 不让他闪退 
	{	
	
		GameDraw();
		SnakeMove();
		Sleep(50);//延迟xx毫秒(减速)
		keycontrol();
		EatFood();
	}


	return 0;
}
```