---
title: C语言实现学生成绩管理系统
date: 2021-05-19 19:41:46
tags:
  - -C语言
categories:
  - C语言
cover: /img/studentManager.png
---

***

相关视频——[【C/C++课程设计】史上最全最详细的学生成绩管理系统上线啦，完成大学课程设计不是问题！_哔哩哔哩 (゜-゜)つロ 干杯~-bilibili](https://www.bilibili.com/video/BV13z4y117qC?p=8)

***

**代码实现**

```c
#define _CRT_SECURE_NO_WARNINGS 1
#include<stdio.h>
#include<conio.h>//从键盘接收一个按键，无序按回车的那种
#include<stdlib.h>
#include<string.h>
//定义学生
typedef struct _Student
{
	
	char name[20];
	int age;
	int stuNum;//学号
	int score;//成绩
}Student;
//定义链表的结点
typedef struct _Node
{
	Student stu;//学生-数据域
	struct _Node* pNext;//指向下一个结点的指针
}Node;
//定义头结点
Node* g_pHead = NULL;
//录入学生信息
void inputStudent()
{
	//创建一个结点-动态开辟
	Node* pNewNode = (Node*)malloc(sizeof(Node));
	pNewNode->pNext = NULL;
	//头插法
	if (g_pHead == NULL)//原来什么也没有
	{
		g_pHead = pNewNode;
	}
	else
	{
		pNewNode->pNext = g_pHead;
		g_pHead = pNewNode;
	}
	printf("请输入学生姓名:\n");
	scanf("%s", pNewNode->stu.name);//name是数组名，不用加&
	printf("请输入学生年龄:\n");
	scanf("%d",&pNewNode->stu.age);
	printf("请输入学生的学号:\n");
	scanf("%d",&pNewNode->stu.stuNum);
	printf("请输入学生的成绩:\n");
	scanf("%d", &pNewNode->stu.score);
	printf("录入完成！\n");
	system("pause");
	system("cls");//清屏
}
//打印学生信息
void printStudent()
{	
	system("cls");
	printf("——————————————------——————————————------\n");
	printf("*\t————————欢迎使用高校学生管理系统——————----\t\n");
	printf("——————————————------——————————————------\n");
	printf("*\t学号\t*\t姓名\t*\t年龄\t*\t成绩*\n");
	printf("——————————————------——————————————------\n");
	//遍历链表
	Node* p = g_pHead;
	while (p != NULL)
	{
		printf("\t%d\t\t%s\t\t%d\t\t%d\t\n",p->stu.stuNum,p->stu.name,p->stu.age,p->stu.score);
		p = p->pNext;
	}
	printf("——————————————------——————————————------\n");
	system("pause");
	system("cls");
}
//保存学生信息
void saveStudent()
{
	system("cls");
	//打开文件
	FILE* fp = fopen("文件路径","w");
	if (fp == NULL)
	{
		printf("打开文件失败。\n");
		return;
	}
	//遍历链表
	Node* p = g_pHead;
	while (p != NULL)
	{
		fwrite(&p->stu, 1,sizeof(Student),fp);
		p = p->pNext;
	}
	//关闭文件
	fclose(fp);
	printf("保存数据成功。\n");
	system("pause");
	system("cls");
}
//读取学生信息
void browerStudent()
{
	//打开文件
	FILE* fp = fopen("C:\\Users\\xuanxuan\\Desktop\\test.txt", "r");
	if (fp == NULL)
	{
		printf("打开文件失败。\n")																																												;
	}
	//读文件
	Student stu;
	while (fread(&stu, 1, sizeof(Student), fp))//只要不是文件末尾就继续读
	{
		//创建一个新结点
		Node* pNewNode = (Node*)malloc(sizeof(Node));
		pNewNode->pNext = NULL;
		memcpy(pNewNode,&stu,sizeof(Student));

		//头插法
		if (g_pHead == NULL)//原来什么也没有
		{
			g_pHead = pNewNode;
		}
		else
		{
			pNewNode->pNext = g_pHead;
			g_pHead = pNewNode;
		}
	}
	//关闭文件
	fclose(fp);
	printf("加载数据成功。\n");
	system("pause");
	system("cls");
}
//统计所有学生人数
int countStudent()
{
	int nCount = 0;
	//遍历链表
	Node* p = g_pHead;
	while (p != NULL)
	{	
		nCount++;
		p = p->pNext;
	}
	return nCount;
}
//查找学生
Node* findStudent()
{
	int nStudent;
	char nName[20];
	printf("请输入要查找的学生学号:\n");
	scanf("%d", &nStudent);

	printf("请输入要查找的学生姓名:\n", nName);
	scanf("%s",nName);
	//遍历链表
	Node* p = g_pHead;
	while (p != NULL)
	{
		if (p->stu.stuNum == nStudent || 0 == strcmp(p->stu.name , nName))
		{
			return p;
		}
		p = p->pNext;
	}
	return NULL;
}
//修改学生信息
void modifyStudent()
{
	int nStunum;
	printf("请输入要修改学生的学号:\n");
	scanf("%d", &nStunum);
	Node* p = g_pHead;
	while (p != NULL)
	{
		if (p->stu.stuNum == nStunum)
		{
			printf("请输入修改学生的姓名 年龄 成绩：\n");
			scanf("%s %d %d", p->stu.name, &p->stu.age, &p->stu.score);
			printf("修改成功。\n");
			break;
		}
		p = p->pNext;
	}
	if (p == NULL)
	{
		printf("没有找到该学生信息。\n");
	}
	system("pause");
	system("cls");
}
//删除学生信息
void deleteStudent()
{
	int nStunum;
	printf("请输入要删除的学生学号。\n");
	scanf("%d",&nStunum);

	Node* p1,*p2;
	//判断是不是头结点
	if (g_pHead->stu.stuNum == nStunum)
	{
		p1 = g_pHead;
		g_pHead = g_pHead->pNext;
		free(p1);
		printf("删除成功。\n");
		system("pause");
		system("cls");
		return;
	}
	//不是头结点
	Node* p = g_pHead;
	while (p->pNext != NULL)
	{
		if (p->pNext->stu.stuNum == nStunum)
		{
			p2 = p->pNext;
			p->pNext = p->pNext->pNext;
			free(p2);
			printf("删除成功。\n");
			system("pause");
			system("cls");
			return;
		}
		p = p->pNext;
		if (p->pNext == NULL)
		{
			break;
		}
		
	}
	if (p->pNext ==NULL)
	{
		printf("查无此人。\n");
	}
	system("pause");
	system("cls");

}
//主菜单
void mainMenu()
{
	printf("——————————————------\n");
	printf("*\t欢迎使用高校学生管理系统*\n");
	printf("——————————————------\n");
	printf("*\t1.录入学生信息\t\t*\n");
	printf("*\t2.打印学生信息\t\t*\n");
	printf("*\t3.保存学生信息\t\t*\n");
	printf("*\t4.读取学生信息\t\t*\n");
	printf("*\t5.统计所有学生人数\t*\n");
	printf("*\t6.查找学生信息\t\t*\n");
	printf("*\t7.修改修生信息\t\t*\n");
	printf("*\t8.删除学生信息\t\t*\n");
	printf("*\t0.退出系统\t\t*\n");
	printf("——————————————------\n");


}
//键盘输入
void keyDown()
{ 
	char ch = _getch();
	switch (ch)
	{
	case '1'://录入
		inputStudent();
		break;
	case '2'://打印
		printStudent();
		break;
	case '3'://保存
		saveStudent();
		break;
	case '4'://读取
		browerStudent();
		break;
	case '5'://统计
		printf("学生总人数为:%d\n", countStudent());
		system("pause");
		system("cls");
		break;
	case '6'://查找
	{
		Node* p = findStudent();
		if (p != NULL)
		{
			printf("学号:%d\t姓名:%s\t年龄:%d\t成绩:%d\n", p->stu.stuNum, p->stu.name, p->stu.age, p->stu.score);
		}
		else
		{
			printf("没有找到该学生。\n");
		}
		system("pause");
		system("cls");
		break;
	}	
	case '7'://修改
		modifyStudent();
		break;
	case '8'://删除
		deleteStudent();
		break;
	case '0'://退出
		exit(0);
		break;
	default:
		printf("输入错误，请重新输入。\n");
		break;
	}
}
int main(void)
{	
	while (1)
	{
		mainMenu();
		keyDown();
	}
	return 0;
}
```

