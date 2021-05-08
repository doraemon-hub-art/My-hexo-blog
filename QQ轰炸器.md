---
title: QQ轰炸器
date: 2021-05-08 17:25:20
tags:
  -	-C语言
categories:
  - C语言
cover: /img/QQ.png
---

***

相关视频——[【C/C++技术教程】QQ轰炸机（两种版本）！程序员带你实现腾讯QQ消息轰炸，瞬间99+让对面防不胜防！_哔哩哔哩 (゜-゜)つロ 干杯~-bilibili](https://www.bilibili.com/video/BV1Vf4y1W7aj?t=3)

***

**注意：群体轰炸，当轰完(群发)完你所选中的分组后，它会继续往下进行，对下一个分组进行发送,连QQ的各种服务号都算上。**

***

**代码实现**

```C
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<stdlib.h>
#include<Windows.h>//使用windows的资源
int main(void)
{	
	int c = 0;
	printf("1.单独轰炸\n");
	printf("2.群体轰炸\n");
	printf("3.退出\n");
	scanf("%d", &c);
	if (c == 1)
	{
		char name[30] = {0};
		int times = 0;
		printf("请输入你要轰炸的对象->\n");
		scanf("%s", &name);
		printf("请输入你要轰炸的次数->\n");
		scanf("%d", &times);
		//HWND-窗口句柄
		//窗口的id-编号 每一个窗口对应一个编号
		HWND qqhwnd;//定义一个变量存储一个窗口的id
		qqhwnd = FindWindowA(NULL,name);//两个信息，一个类名称，一个标题
		//发送消息
		//向某一个窗口发送消息 鼠标-键盘-消息
		//将要发送的消息复制到全局剪贴板
		for (int i = 0; i < times; i++)
		{
			SendMessageA(qqhwnd, WM_PASTE,0,0);
			SendMessageA(qqhwnd, WM_KEYDOWN,VK_RETURN,0);
		}
		//在吗？
	}
	else if (c == 2)
	{
		while (1)
		{
			//群体轰炸
			HWND qqhwnd;
			//得到QQ主界面的窗口ID
			qqhwnd = FindWindowA(NULL, "QQ");
			MoveWindow(qqhwnd, 0, 0, 800, 800, true);
			//1.选中主界面
			//2.TAB TAB
			//3.不断的按回车和下-打开一个对话框
			//4.粘贴
			//5.发送
			//5.关闭对话框

			SetForegroundWindow(qqhwnd);//设置某一个窗口为最前-就是选中主界面
			keybd_event(VK_TAB, 0, 0, 0);//按下TAB键
			Sleep(50);//慢一点
			keybd_event(VK_TAB, 0, 2, 0);//弹起TAB键
			Sleep(50);

			keybd_event(VK_TAB, 0, 0, 0);//按下TAB键
			Sleep(50);//慢一点
			keybd_event(VK_TAB, 0, 2, 0);//弹起TAB键
			Sleep(50);

			//不断的按回车和下 打开对话框
			while (1)
			{
				//回车
				keybd_event(VK_RETURN, 0, 0, 0);
				Sleep(50);
				keybd_event(VK_RETURN, 0, 2, 0);
				Sleep(50);
				//下键
				keybd_event(VK_DOWN, 0, 0, 0);
				Sleep(50);
				keybd_event(VK_DOWN, 0, 2, 0);
				Sleep(50);

				if (qqhwnd != GetForegroundWindow())
				{
					break;
				}
			}
			//粘贴
			keybd_event(VK_CONTROL, 0, 0, 0);
			Sleep(50);
			keybd_event('V', 0, 2, 0);
			Sleep(50);

			keybd_event('V', 0, 0, 0);
			Sleep(50);
			keybd_event(VK_CONTROL, 0, 2, 0);
			Sleep(50);

			//发送
			keybd_event(VK_RETURN, 0, 0, 0);
			Sleep(50);
			keybd_event(VK_RETURN, 0, 2, 0);
			Sleep(50);

			//关闭对话框
			keybd_event(VK_ESCAPE,0, 0, 0);
			Sleep(50);
			keybd_event(VK_ESCAPE,0, 2, 0);
			Sleep(50);
		}
	}
	else if (c == 3)
	{
		exit(0);
	}
	return 0;
}
```

