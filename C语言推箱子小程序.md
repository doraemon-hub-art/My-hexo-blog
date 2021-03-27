---
title: C语言推箱子小程序
date: 2021-03-27 00:03:07
tags:
	- -C语言
categories:
	- C语言
cover: /img/jianpan3.png
---

参考视频https://www.bilibili.com/video/BV1By4y1a79o?t=4428

**包括黑窗界面和图形界面**

**BUG：当人物进入到目的地的时候就动不了了**

```c
#include<stdio.h>
#include<stdlib.h>
//这个库函数不是C 语言标准的，在VS上可以直接用，在Linux上就不行。
#include<conio.h>
//使用布尔类型
#include<stdbool.h>
//使用图形界面-图形界面头文件（需要安装）
#include<graphics.h>
//推箱子
//知识点：数组 、函数、
//开发环境 vs2019
//准备地图数据 用二维数组来存储
//表示——空地 0 墙 1 目的地 2 箱子 3 玩家 4  
//这两个是动态变化的 箱子+目的地 5 玩家+目的地 6 
//难点在于判断移动导致的变化

#define SPACE	0
#define WALL	1
#define DEST	2
#define BOX		3
#define PLAYER	4
#define ROW 10
#define COL 10
//当前所在关卡
int level = 0;
//变成3纬数组 可以存多个地图
int map[3][ROW][COL] = 
{
	//设计地图样式
	//map1
	{
		{0,0,0,0,0,0,0,0,0,0},
		{0,0,0,1,1,1,0,0,0,0},
		{0,0,0,1,2,1,0,0,0,0},
		{0,0,0,1,3,1,1,1,1,0},
		{0,1,1,1,0,3,0,2,1,0},
		{0,1,2,3,4,0,1,1,1,0},
		{0,1,1,1,1,3,1,0,0,0},
		{0,0,0,0,1,2,1,0,0,0},
		{0,0,0,0,1,1,1,0,0,0},
		{0,0,0,0,0,0,0,0,0,0}
	},
	//map2
	{
		{0,0,0,0,0,0,0,0,0,0},
		{0,0,1,1,0,0,1,1,0,0},
		{0,1,0,2,1,1,2,0,1,0},
		{1,0,0,0,0,0,0,0,0,1},
		{1,0,0,3,0,3,0,0,0,1},
		{1,0,0,0,4,0,0,0,0,1},
		{0,1,0,3,0,3,0,0,1,0},
		{0,0,1,2,0,0,2,1,0,0},
		{0,0,0,1,0,0,1,0,0,0},
		{0,0,0,0,1,1,0,0,0,0}
	},
	//map3
    {
		{0,0,0,0,0,0,0,0,0,0},
		{0,1,1,1,1,1,1,1,1,0},
		{0,1,2,0,0,0,0,2,1,0},
		{0,1,0,0,0,0,0,0,1,0},
		{0,1,0,3,0,3,0,0,1,0},
		{0,1,0,0,4,0,0,0,1,0},
		{0,1,0,3,0,3,0,0,1,0},
		{0,1,2,0,0,0,0,2,1,0},
		{0,1,1,1,1,1,1,1,1,0},
		{0,0,0,0,0,0,0,0,0,0}
	}
};
//定义一个图片的数组int image
IMAGE img[6];
//加载图片
void loadImg()
{
	for (int i = 0; i < 6; i++)
	{	
		char temFileName[50] = { 0 };
		sprintf_s(temFileName,"./images/%d.bmp", i);
		loadimage(img + i, temFileName, 63, 63); //项目属性-高级-字符集-使用多字符字符集
	}	
}
void DrawMap()
{
	for (int i = 0; i < 10; i++)
	{
		for (int k = 0; k < 10; k++)
		{
			switch (map[level][i][k])
			{
			case SPACE:
				putimage(k * 63, i * 63, img + 0);
				break;
			case WALL:
				putimage(k * 63, i * 63, img + 1);
				break;
			case DEST:
				putimage(k * 63, i * 63, img + 2);
				break;
			case BOX:
				putimage(k * 63, i * 63, img + 3);
				break;
			case PLAYER:
				putimage(k * 63, i * 63, img + 4);
				break;
			case BOX + DEST:
				putimage(k * 63, i * 63, img + 5);		
				break;
			case PLAYER + DEST:
				putimage(k * 63, i * 63, img + 5);
				break;
			default:
				printf("%d ", map[level][i][k]);
				break;
			}
		}
		printf("\n");
	}
}
void show()
{
	for (int i = 0; i < 10; i++)
	{
		for (int k = 0; k < 10; k++)
		{	
			switch (map[level][i][k])
			{
			case SPACE:
				printf("  ");
				break;
			case WALL:
				printf("▓");
				break;
			case DEST:
				printf("☆");
				break;
			case BOX:
				printf("★");
				break;
			case PLAYER:
				printf("♂");
				break;
			case BOX + DEST:
				printf("◇");
				break;
			case PLAYER + DEST:
				printf("♀");
				break;
			default:
				printf("%d ", map[level][i][k]);
				break;
			}
		}
		printf("\n");
	}
}
void pushBox()
{
	//找到玩家所在的下标
	//地图里面哪些数据有可能是玩家
	//PLAYER PLAYER+DEST
	int i = 0; 
	int k = 0;
	for ( i = 0; i < 10; i++)
	{
		for ( k = 0; k<10; k++)
		{
			if (map[level][i][k] == PLAYER)
			{
				goto end;
				break;
			}
		}
	}
end:;//goto到这里
	//获取键盘按键 -  _getch()-一触即发不需要按回车 getchar()-输入之后需要按回车
	char key = _getch();
	//printf("%d %c\n", key, key);
	switch (key)
	{
	case 'w':
	case 'W':
	case 72://向上移动
		//什么情况下 玩家才能移动 才能推箱子？
		//玩家的前面是空地(目的地)、玩家的前面是箱子（箱子的前面是什么） 可以动
		//如果玩家的前面是空地
		if (map[level][i- 1][k] == SPACE || map[level][i - 1][k] == DEST)
		{
			map[level][i - 1][k] += PLAYER;
			map[level][i][k] -= PLAYER;
		}
		else if(map[level][i-1][k] == BOX)//玩家的前面是箱子
		{
			if (map[level][i - 2][k] == SPACE || map[level][i - 2][k] == DEST)//箱子的前面是空地或者是目的地
			{
				map[level][i - 2][k] += BOX;
				map[level][i - 1][k] += (PLAYER - BOX);
				map[level][i][k] -= PLAYER;
			}
		}
		break;
	case 's':
	case 'S':
	case 80://向下移动
		if (map[level][i + 1][k] == SPACE || map[level][i + 1][k] == DEST)
		{
			map[level][i + 1][k] += PLAYER;
			map[level][i][k] -= PLAYER;
		}
		else if (map[level][i + 1][k] == BOX)//玩家的前面是箱子
		{
			if (map[level][i + 2][k] == SPACE || map[level][i + 2][k] == DEST)//箱子的前面是空地或者是目的地
			{
				map[level][i + 2][k] += BOX;
				map[level][i + 1][k] += (PLAYER - BOX);
				map[level][i][k] -= PLAYER;
			}
		}
		break;
	case 'a':
	case 'A':
	case 75://向左移动
		if (map[level][i][k - 1] == SPACE || map[level][i][k - 1] == DEST)
		{
			map[level][i][k - 1] += PLAYER;
			map[level][i][k] -= PLAYER;
		}
		else if (map[level][i][k - 1] == BOX)//玩家的前面是箱子
		{
			if (map[level][i][k - 2] == SPACE || map[level][i][k - 2] == DEST)//箱子的前面是空地或者是目的地
			{
				map[level][i][k - 2] += BOX;
				map[level][i][k - 1] += (PLAYER - BOX);
				map[level][i][k] -= PLAYER;
			}
		}
		break;
	case 'd':
	case 'D':
	case 77://向右移动
		if (map[level][i][k + 1] == SPACE || map[level][i][k + 1] == DEST)
		{
			map[level][i][k + 1] += PLAYER;
			map[level][i][k] -= PLAYER;
		}
		else if (map[level][i][k + 1] == BOX)//玩家的前面是箱子
		{
			if (map[level][i][k + 2] == SPACE || map[level][i][k + 2] == DEST)//箱子的前面是空地或者是目的地
			{
				map[level][i][k + 2] += BOX;
				map[level][i][k + 1] += (PLAYER - BOX);
				map[level][i][k] -= PLAYER;
			}
		}
		break;
	default:
		break;
	}
}
//什么情况下才过关呢，当前关卡没有箱子了 就通关了
bool Judge()
{
	for (int i = 0; i < ROW; i++)
	{
		for (int k = 0; k < COL; k++)
		{
			if(map[level][i][k] == BOX)
			{
				return false;
			}
		}
	}
	return true;
}
int main(void)
{	
	//设置黑窗口的大小
	system("mode con  cols=30 lines=20");//设置cols和lines数值的时候不能有空格，否则会报错。
	//创建图形界面窗口
	//参数 窗口的宽度 高度 SHOWCONSOLE表示同时显示控制台和控制台
	initgraph(ROW*63,COL*63,SHOWCONSOLE);
	//Easyx只能用于C++,所以源文件后缀改为.cpp
	loadImg();
	while (1)
	{	
		system("cls");
		show();
		DrawMap();
		//判断是否过关
		if (Judge())
		{
			//切换关卡
			level++;//最大只有三关，超过了就数组越界了
			if (level > 2)
			{
				exit(0);
			}
			printf("按任意键进入下一关......\n");
		}
		pushBox(); 
	}
	
	//getchar();——防止闪退的，加了while 1就不需要了
	return 0;
}
```

