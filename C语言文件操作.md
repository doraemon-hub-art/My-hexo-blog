---
title: C语言文件操作
date: 2021-05-18 16:37:38
tags:
  - -C语言
categories:
  - C语言
cover: /img/files.png
---

***

相关视频——[C语言精华——C语言文件操作，文件打开、关闭、读取、定位如何操作？为你逐一讲解文件操作标准库函数_哔哩哔哩 (゜-゜)つロ 干杯~-bilibili](https://www.bilibili.com/video/BV1F54y1r7ww?from=search&seid=15927253292233017726)

***

**文件分类：**

一种是文本文件，一种是二进制文件。

- 文本文件：保存的时候，没一个字符对应一个字节。
- 二进制文件：按照二进制编码保存的文件。

**文件操作：**

# **打开文件**

 打开文件fopen("文件路径"，"打开方式")

参数：-(百度百科)

![](/images/C语言文件操作.assets/20210517213436.png)

(选中函数按F1打开msdn文档）

打开文件成功返回一个文件指针，打不开返回 NULL。

```C
#define _CRT_SECURE_NO_WARNINGS 1
#include<stdio.h>
int main(void)
{	
	FILE* fp = fopen("C:\\Users\\XX\\Desktop\\test.txt","r");
	if (fp == NULL)
	{
		printf("打开文件失败\n");
	}
	char ch = fgetc(fp);
	while ((ch = fgetc(fp)) != EOF)
	{
		printf("%c",ch);
	}
	fclose(fp);
	return 0;

}
```

# 关闭文件

```c
fclose();
```



# 读取文件

## fgetc

```c
char ch  = fgetc();//返回一个字符，一个字符一个字符的读取。
```

 打开文件之后，到关闭文件之前操作，会有一个文件指针定位到你当前操作到哪里了，读取了一个字节，文件指针就会继续往后偏移。

***

**读取完会将文件指针移动到下一个字符。**

***

可以使用循环将全部文本全部内容读取。

## fgets

读取一行fgets()

```C
char str[200];
fgets(str,200,fp);
printf("%c",str);
```

也可以通过循环将内容一行一行的读取出来。

## fread

fread想读多少读多少

fread(str存到哪,每个元素大小，读几个，文件)；

返回实际读取的大小

```C
fread(str,1,10,fp);
```

***

清零

```C
char str[200] = {0};//初始化
	   或
memset(str,0,sizeof(str);
       或
int n = fread(str,1,10,fp);
str[n] = '\0';
```

# 写入文件

## fputc

fputc('内容',文件);

## fputs

写入一个字符串

fputs();

```C
char* str = "xxxxxxxxxxxxxxxxxxxxxxx\r\n";
			\r\n回车
fputs(str,fp);
```

## fwrite

fwrite想写多少写多少

```C
int num = 123124;
fwrite(&num,sizeof(num),1,fp);
第一个参数类型是void* 可以转化为任意类型
```

```c
#define _CRT_SECURE_NO_WARNINGS 1
#include<stdio.h>
typedef struct Person
{
	char name[20];
	char sex[4];
	int age;
}_Person;
int main(void)
{
	FILE* fp = fopen("C:\\Users\\XX\\Desktop\\test.txt", "r+");
	if (fp == NULL)
	{
		printf("打开文件失败\n");
	}
	Person p1 = {"张三","男",20};
	fwrite(&p1, 1, sizeof(p1), fp);
	fclose(fp);
	return 0;

}
```

![image-20210518120745992](/images/C语言文件操作.assets/image-20210518120745992.png)

# 文件定位

文件指针定位

## fseek

fseek(fp,0,SEEK_SET)

能够移动文件指针

可以指定文件从哪里开始读取

```c
	char str[200];
	fseek(fp, 10, SEEK_SET);
	fread(str,1,100,fp);
```

在当前位置再往后移动x个位置

```c
 fseek(fp,10,SEEK_CUR);
```

读取文件最后一行

```c
fseek(fp,0,SEEK_END);
```

```c
#define _CRT_SECURE_NO_WARNINGS 1
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
typedef struct Person
{
	char name[20];
	char sex[4];
	int age;
}_Person;
int main(void)
{
	FILE* fp = fopen("C:\\Users\\XX\\Desktop\\test.txt", "r+");
	if (fp == NULL)
	{
		printf("打开文件失败\n");
	}
	//读取文件最后一行
	fseek(fp,0,SEEK_END);
	//反着读
	fseek(fp, -1, SEEK_END);
	char ch = 0;
	int length = 0;
	while (fread(&ch, 1, 1, fp))
	{
		if (ch == '\n')
		{
			break;
		}
		fseek(fp,-2,SEEK_CUR);
		length++;//统计退了多少格
	}
	printf("length = %d\n", length);
	fseek(fp, -length, SEEK_END);
	char* buffer = (char*)malloc(sizeof(char) * length + 1);//多一个空间存储字符串终止符
	memset(buffer,0,length+1);
	fread(buffer, 1, length,fp);
	printf("%s\n",buffer);
	fclose(fp);
	//释放内存
	free(buffer);
	return 0;
}
```

## rewind

重置文件指针，返回到文件的开头。

## ftell

返回当前指针位置。

***

文件指针移动了多少个字节，该文件的大小就是多少。

```c
	rewind(fp);//重置文件指针到开头
	fseek(fp, 0, SEEK_END);//将文件指针定位到结尾
	int nSize = ftell(fp);//文件指针偏移量
	printf("%d\n", nSize);
```

***









