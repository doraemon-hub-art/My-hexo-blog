---
title: 如果你准备学习C++,并且有C语言的基础，我希望你能简单的过一遍知识点。
date: 2021-06-10 20:10:43
tags: 
  - -C++
categories:
  - C++
cover: /img/c++.png
---

***

相关视频——[黑马程序员匠心之作|C++教程从0到1入门编程,学习编程不再难_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1et411b73Z?p=1)（1-83）

我知道这个视频早已经被很多人学习并且记录笔记，但是我还是想再过一遍前面的基础知识点，所以我这个笔记会非常的简洁，适合有C语言基础的小伙伴进行简单的基础知识复习，好尽快投入到C++的学习中。

***

在基础知识部分，好像只有头文件的引用和输入输出函数发生了变化。

头文件下加入using namespace std;

#include<stdio.h>——>#include<iostream>

printf——>cout

scanf——>cin

C++有字符串类型string,这是C语言所不具备的。

***

**下面就让我们开始吧！**

![](/images/如果你准备学习C.assets/晃动的小耗子.gif)

## Hello C++

### 第一个程序

```c++
#include <iostream>
using namespace std;
int main(void)
{
	cout << "Hellow world" << endl;
	system("pause");
	return 0;
}
```

### 注释

方便自己和他人阅读,不会被程序执行。

```c++
//单行
```

```c++
/*多行注释*/
```

### 变量

**作用：**给一段指定的内存空间起名，方便操作这段内存。

**语法：**数据类型  变量名 = 初始值；

```c++
int a = 10;
cout << "a = "<< a << endl;
```

### 常量

**作用：**用于记录程序中不可更改的数据。

C++定义常量的两种方法

1.#define宏定义

```c++
#define 常量名 常量值
```

通常在文件上方定义,表示一个常量。

2.const修饰的变量

```c++
const 数据类型 常量名 = 常量值
```

通常在变量定义之前加关键字const,修饰该变量为常量，不可修改。

**示例**：

```c++
#define day 7//是不可修改的值，一旦修改就会报错

const int month = 30;
```

### 关键字

**作用:**关键字是C++中预先保留的单词（标识符）

在定义变量或常量的时候不要使用关键字。

![image-20210608180216685](/images/如果你准备学习C.assets/image-20210608180216685.png)

来源（菜鸟教程——[C++ 的关键字（保留字）完整介绍 | 菜鸟教程 (runoob.com)](https://www.runoob.com/w3cnote/cpp-keyword-intro.html)）

### 标识符命名规则

**作用：**C++规定给标识符(变量、常量)命名时，有一套自己的规则

- 标识符不能是关键字
- 标识符只能由字母、数字、下划线组成
- 第一个字符必须为字母或者下划线
- 标识符中字母区分大小写
- (建议：给标识符命名的时候，争取做到见名知意，方便自己和他人阅读。)

## 数据类型

C++规定在创建一个变量或者常量的时候，必须要指定出相应的数据类型，否则无法给该变量分配内存空间。

### 整型

**作用：**整型变量表示的是整型类型的数据。

C++中能够表示整型的类型有以下几种方式，**区别在于占用的内存空间不同。**

![image-20210608181144671](/images/如果你准备学习C.assets/image-20210608181144671.png)

### sizeof关键字

**作用：**统计数据类型所占空间的大小。

**语法**：

```c++
sizeof(数据类型/变量);
```

**示例**：

```c++
#include <iostream>
using namespace std;
int main(void)
{
	cout << "int类型所占空间的大小是：" <<sizeof(int)<< endl;
	system("pause");
	return 0;
}
```

### 实型(浮点型)

**作用：**用于表示小数。

浮点型分为两种-单精度float-双精度double。

两者的区别在于有效数字的表示范围不一样。

![image-20210608181700530](/images/如果你准备学习C.assets/image-20210608181700530.png)

```c++
float f1 = 3.14f;//编译器会默认把一个小数当做双精度
//默认情况下输出一个小数会显示出6位有效数字
//例如：下面这个f1只输出到6
float f1 = 3.1234567f;
```

### 字符型

**作用：**字符型变量用于显示单个字符。

语法：

```c++
char sb = 'a';
/*注意：
显示字符型变量时用单引号括起来，不是双引号。
单引号内只能有一个字符，不可以是字符串。*/
```

- C和C++中字符型变量只占1个字节。
- 字符型变量并不是把所有的字符本身放到内存中存储，而是将对饮的ASCII编码放入到存储单元中。

### 转义字符

**作用：**用于表示一些不能显示出来的ASCII字符。

![image-20210608183348489](/images/如果你准备学习C.assets/image-20210608183348489.png)

(图片来源——w3cschoolw3cschool[C语言转义字符 (biancheng.net)](http://c.biancheng.net/view/1769.html))

```c++
cout << “hello world\n”<< endl;
```

### 字符串

**作用：**用于表示一串字符串。

**两种风格：**

1.C风格字符串

要用双引号括起来

```c++
char 变量名[] = "字符串值";
```

```c++
char str1[] = "hello world";
```

2.C++风格字符串

需要加入头文件#include <string>

```c++
string 变量名 = "字符串值";
```

```c++
string st2 = "hellow world";
```

### 布尔类型bool

**作用:**布尔类型数据代表真或假的值。

bool类型只有两个值：

- true——真（1）
- false——假（0）

**bool类型占1个字节大小**

示例：

```c++
bool flag = true;
```

### 数据 输入

**作用：**用于从键盘获取数据

**关键字：**cin

**语法**：

```c++
cin >> 变量
```

```c++
int a = 0;
cin >>a;
```

## 运算符

**作用:**用于代码的运算。

![image-20210608193429732](/images/如果你准备学习C.assets/image-20210608193429732.png)

### 算数运算符

**作用：**用于处理四则运算

![image-20210608193743764](/images/如果你准备学习C.assets/image-20210608193743764.png)

```c++
+ - * / % ++ -- 
两个整数相除结果还是整数
两个小数相除结构还是小数
两个数相除除数不可以为0
```

```c++
	前置递增++a——先让变量+1然后再进行表达式运算
    后置递增a++——先进行表达式运算然后变量再+1
    递减同理
```

###  赋值运算符

**作用：**用于将表达式的值赋给变量。

![](/images/如果你准备学习C.assets/image-20210608194415559.png)

**示例：**

```c++
int a = 1;
a *=2;//意思就是就是a = a *2;
```

### 比较运算符

**作用：**用于表达式的比较，并返回一个真值或假值。

![image-20210608194714016](/images/如果你准备学习C.assets/image-20210608194714016.png)

**示例**：

```c++
	
int a = 4;	
int b = 3;	
cout << (a < b)<<endl;//真返回1，假0
```

### 逻辑运算符

**作用：**用于根据表达式的值返回真值或假值。

![image-20210608194954139](/images/如果你准备学习C.assets/image-20210608194954139.png)

**示例：**

```c++
int a = 9;
int b = 10;
int c = 0;
cout<< !a <<endl;//0
cout<< !!a <<endl;//1
cout<< (a && b) <<endl;//1
cout<< (a && c) <<endl;//0
cout<< (a || c) <<endl;//1
```

## 程序流程结构

C/C++支持最基本的三大基本程序运算结构:**顺序结构、选择结构、循环结构。**

- 顺序结构：程序按顺序执行，不发生跳转。
- 选择结构：依据条件是否满足,有选择的执行相应代码。
- 循环结构：依据条件是否满足，循环多次指定某段代码。

### 选择结构

#### **if语句**

**作用：**执行满足条件的语句。

- 单行格式if语句

```c++
if(条件)
{
	//条件满足执行的语句
}
```

- 多行格式if语句

```c++
if(条件){    //条件满足执行的语句}else{    //条件不满足执行的语句}
```

- 多条件if语句

```c++
if(条件1)
{
    //条件1满足执行的语句
}
else if(条件2)
{
    //条件2满足执行的语句
}
......
else
{
    //都不满足执行的语句
}
```

- 嵌套if语句

```c++
if()
{
    if()
}
else
{
    
}
```

#### 三目运算符

**作用：**通过三目运算符实现简单的判断

**语法：**

```c++
表达式1？表达式2：表达式3
```

**解释：**

如果1为真，则结果为表达式2的值。

如果1为假，则结果为表达式3的值。

**示例：**

```c++
	int a = 9;
	int b = 10;
	int c = 0;
	c = a > b ? a : b;
	cout << c << endl;//结果为10
```

#### switch语句

**作用：**执行多条件分支语句

**语法：**

```c++
switch(表达式)
{
        case 结果1：
            执行语句;
            break;
        ......
        default：
            执行语句;
        	break;
}
```

### 循环结构

#### while循环语句

**作用：**满足循环条件，执行循环语句

**语法：**

```c++
while(循环条件){    循环语句}
```

**解释：**只要满足循环条件的结果为真，就执行循环语句。

##### 猜数字练习

```c++
#include <iostream>
#include<string>
using namespace std;
int main(void)
{
	int num = rand() % 100;
	cout << num << endl;
	int puT = 0;
	cout << "请你猜一下这个数是多少\n" << endl;
	while ((cin >> puT))
	{
		if (puT > num)
		{
			cout << "猜大了\n" << endl;
		}
		else if (puT <= num / 2)
		{
			cout << "太小了\n" << endl;
		}
		else if (puT >= num / 2 && puT < num)
		{
			cout << "再大一点\n" << endl;
		}
		else if (num == puT)
		{
			cout << "猜对了\n" << endl;
			break;
		}
	}
	system("pause");
	return 0;
}
```

#### do-while循环语句

**作用：**满足循环条件，执行循环语句。

**语法：**

```c++
do{    循环语句}while(循环条件)
```

**注意：**与while的区别在于do-while会先执行一次循环语句，再判断循环条件。

##### 水仙花数练习

```c++
#include <iostream>
using namespace std;
int main(void)
{
	int ge = 0;
	int shi = 0;
	int bai = 0;
	int i = 100;
	do
	{
		ge = i % 10;
		shi = (i / 10) % 10;
		bai = i / 100;
		if (i == ge * ge * ge + shi * shi * shi + bai * bai * bai)
		{
			cout << i << endl;
		}
		i++;
	} while (i < 1000);
	system("pause");
	return 0;
}
```



#### for循环语句

**作用：**满足循环条件，执行循环语句

**语法：**

```c++
for(起始条件;条件表达式;末尾循环体){    循环语句}
```

**示例：**

```c++
#include<iostream>
using namespace std;
int main(void)
{    
    for(int i = 0;i < 10;i++)   
    {    
        
    }    
    return 0;
}
```

#### 敲桌子练习

##### 是7的倍数、各位有7、十位有7

```c++
#include<iostream>
using namespace std;
int main(void)
{	
    for (int i = 1; i < 100; i++)	
    {		
        int ge = i % 10;		
        int shi = (i /10)% 10;		
        if (i % 7 == 0 || ge == 7 || shi == 7)		
        {			
            cout << i << endl;		
        }	
    }
}
```

#### 嵌套循环

**作用：**在循环体中再嵌套一层循环，解决一些实际问题。

##### 打印10*10的正方形

```c++
	#include<iostream>
	using namespace std;
	int main(void)
	{
		for (int i = 0; i < 10; i++)
		{
			for (int i = 0; i < 10; i++)
			{
				cout << "* ";
			}
			cout << endl;
		}
	}
```

##### 乘法口诀表练习

```c++
#include<iostream>
using namespace std;
int main(void)
{	
    for (int i = 1; i < 10; i++)	
    {			
        for (int j = 1; j <= i; j++)		
        {			
            cout << i <<"*"<< j<<"="<< i* j<<" ";		
        }		
        cout << endl;	
    }
    system("pause");
    return 0;
}
```

#### 跳转语句

#### break语句

**作用：**用于跳出选择结构或者循环结构。

break使用的时机：

- 出现在switch语句中，作用是终止case并跳出swtich
- 出现在循环语句中，作用是跳出当前的循环语句
- 出现在嵌套语句中，跳出最近的内层循环语句

#### continue语句

作用：在循环语句中，跳过本次循环中余下尚未执行的语句，继续执行下一次循环。

#### goto语句

**作用：**可以无条件跳转语句

**语法：**goto标记；

**解释：**如果标记的名称存在，执行到goto语句的时候，会跳转到标记的位置。

```c++
goto sb;......sb:......
```

## 数组

### 概述

所谓数组就是一个集合，里面存放了相同类型的数据元素

**特点1**：数组中的每个数据元素都是相同的数据类型。

**特点2**：数组是由连续的内存位置组成的。

### 一维数组

#### 定义

```c++
数据类型 数组名[数组长度];数据类型 数组名[数组长度] = {值1，值2......};数据类型 数组名[] = {值1，值2......};;
```

#### 数组名的用途

1.可以统计整个数组在内存中的长度

2.可以获取数组在内存中的首地址

#### 输出最重的一只小猪的体重

```c++
#include<iostream>
using namespace std;
int main(void)
{	
    int temp = 0;	
    int Weight[5] = { 300,250,200,400,450 };	
    for (int i = 0; i < 5; i++)	
    {		
        if (Weight[i] > temp)		
        {			
            temp = Weight[i];		
        }	
    }	
    cout << "最重的小猪是" << temp << "kg";	
    return 0;
}
```

#### 数组元素逆置

```c++
#include<iostream>
using namespace std;
int main(void)
{ 	
    int temp = 0;	
    nt nums[5] = { 1,2,3,4,5};	
    int start = 0;	
    int end = sizeof(nums)/sizeof(nums[0]) -1 ;	
    while (start < end)	
    {		
        temp = nums[start];		
        nums[start] = nums[end];		
        nums[end] = temp;		
        end--;		
        start++;	
    }	
    for (int i = 0; i < 5; i++)
    {		
        cout << nums[i];	
    }	
    return 0;
}
```

#### 冒泡排序

**作用：**最常用的排序算法，对数组内元素进行排序

1. 比较相邻两个元素，如果第一个比第二个大就交换他们的位置
2. 每一对相邻元素做同样的工作，整型完毕后，找到第一个最大值。
3. 重复以上的步骤，每次比较次数-1，知道不需要比较

**示例**:

```c++
//排列这个数组{7,5,2,4,9,8,6,7,1}
#include<iostream>
using namespace std;
int main(void)
{ 
	//排序的总轮数=元素个数-1
	//每轮对比的次数 = 元素个数- 排序轮数 
	int nums[9] = { 7,5,2,4,9,8,6,7,1 };
	for (int i = 0; i < 8 ; i++)
	{
		//内层循环对比
		for (int j = 0; j < 9 - i-1; j++)
		{
			//第一个数比第二个数大就交换他们两个的位置
			int temp = 0;
			if (nums[j] > nums[j + 1])
			{
				temp = nums[j+1];
				nums[j + 1] = nums[j];
				nums[j] = temp;
			}
		}
	}
	for (int i = 0; i < 9; i++)
	{
		cout << nums[i];
	}
	return 0;
}

```

### 二维数组

二维数组就是在一维数组的基础上多加一个维度，就是在一维数组里面存储一维数组。

**定义:**

```c++
数据类型 数组名[行][列];
数据类型 数组名[行][列] = {{数据1，数据2}，{数据3，数据4}};
数据类型 数组名[行][列] = {数据1，数据2，数据3，数据4};
数据类型 数组名[][列] = {数据1，数据2，数据3，数据4};
```

**建议：**以上4种定义方式，利用第二种更加直观，提高代码的可读性。

#### 数组名

- 查看二维数组所占内存空间
- 获取二维数组首地址

#### 考试成绩统计练习

|      | 语文 | 数学 | 英语 |
| ---- | ---- | ---- | ---- |
| 甲   | 50   | 40   | 60   |
| 乙   | 20   | 10   | 30   |
| 丙   | 70   | 80   | 90   |

分别输出三个人的总成绩

```c++
#include<iostream>
using namespace std;
int main(void)
{	
    int score[3][3] = { {60,50,40},{10,20,30},{70,80,90} };	
    //嵌套循环解决	
    for (int i = 0; i < 3; i++)
    {			
        int temp = 0;	
        for (int j = 0; j < 3; j++)		
        {			
            temp += score[i][j];		
            j++;		
        }		
        cout << temp << endl;	
    }
}
```

## 函数

### 概述

**作用：**将一段经常使用的代码封装起来，减少重复代码。

一个较大的程序，一般分为若干个程序块，每个模块实现特定的功能。

###  函数的定义

函数的几个要素
返回值类型，函数名 ，参数，函数体语句，return表达式

**语法：**

```c++
返回值类型 函数名(参数列表)
{
    函数语句;
    return 表达式;
}
```

### 函数的调用

**功能**：使用定义好的函数

**语法：**函数名(参数)

### 值传递

- 就是函数调用时将参数值传给形参
- 值传递时，如果形参发生变化，并不会影响到实参

### 函数的常见样式

无参无返、有参无返、无参有返、有参有返

### 函数的声明

**作用**：告诉编译器函数名称及如何调用函数。函数的实际主体可以单独定义。

函数的声明可以有很多次，定义只能有一次。

```c++
//声明
int max(int a,int b);
//定义
int max(int a ,int b)
{
    return a+b;
}
```

### 函数的分文件编写

**作用：**让代码结构更加清晰

就是在.h的头文件里面放函数声明，函数的定义放到.c文件里

## 指针

### 概念

指针的作用:可以通过指针间接访问内存。

- 内存编号是从0开始记录的，一般用16进制数字标识。
- 可以利用指针变量保存地址。

### 指针变量的定义和使用

**指针变量定义语法**：数据类型+变量名

### 指针所占内存空间

在32位操作系统下无论是什么类型的指针，都占4个字节的内存空间。

### 空指针

**空指针：**指针变量指向内存中编号为0的空间

**用途：**初始化指针变量

**注意：**空指针指向的内存空间是不可以访问的

```c++
int* p = NULL;
```

### 野指针

指针变量指向非法的内存空间。

### const修饰指针

const修饰指针有3种情况 

1. const修饰指针---常量指针
2. const修饰常量---指针常量
3. const既修饰指针，又修饰常量、

```c++
const修饰的是指针，指针指向可以改，指针指向的值不可以改
    const int* p1 = &a;
const修饰的是常量，指针指向不可以改，指针指向的值可以更改
    int* const p2 = &a;
const既修饰指针，又修饰常量，指针的指向和指针指向的值都不可以改变
    const int* const p = &a;
```

### 指针和数组

**作用：**利用指针访问数组元素

```c++
int arr[] = {1,2,3,4};
int* p = arr;
```

### 指针和函数

**作用：**利用指针作函数的参数，可以修改实参的值。

——**传(址)引用**

### 指针、数组、函数

封装一个函数，利用冒泡排序，实现对整型数组的升序排列

```c++
#include<iostream>
using namespace std;
void PopSort(int* a,int len)
{
	for (int i = 0; i < len - 1; i++)
	{
		for (int j = 0; j < len-i - 1; j++)
		{
			int temp = 0;
			if (a[j] > a[j + 1])
			{
				temp = a[j];
				a[j] = a[j + 1];
				a[j + 1] = temp;
			}
		}
	}
}
int main(void)
{	
	int arry[5] = { 6,2,4,8,5 };
	PopSort(arry, 5);
	for (int i = 0; i < 5; i++)
	{
		cout << arry[i];
	}
	return 0;
}
```

## 结构体

### 概念

​	结构体属于用户自定义的数据类型，允许用户存储不同的数据类型。

### 定义和使用

**语法**：

```c++
struct 结构体名称
{
    结构体成员列表
};
```

通过结构体创建变量的方式有三种

- struct 结构体名 变量名
- struct 结构体名 变量名 = （成员1值，成员2值......)
- 定义结构体时顺便创建变量

**示例**：

```c++
struct Student
{
  string name;
  int age;
  int score;
};
```

### 结构体数组

**作用：**将自定义的结构头放入到数组中方便维护

**语法**:

```c++
struct 结构体名 数组名[元素个数]=  {{}，{}...{}};
```



### 结构体指针

**作用**：通过指针访问结构体中的成员

利用操作符->可以通过结构体指针访问结构体属性

```c++
struct Student s1;
struct Student* p = &s1;
p->score = 10;
```

###  结构体嵌套结构体

**作用**：结构体中的成员可以是另一个结构体

**例如**:每个老师辅导一个学员，一个老师的结构体中，记录一个学生的的结构体

### 结构体做函数参数

**作用：**将结构体作为参数向函数中传递

传递方式有两种

同上函数参数-指针

- 值传递-无法改变实参
- 地址传递-可以改变实参

### 结构体中const使用场景

**作用**：用const来防止误操作

```c++
void ChangeInformation(const struct student* stu1)
{
    加了const就无法改变该结构体内的信息
}
```

### 结构体案例

 每个老师带三个学生

```c++
#include<iostream>
#include<string>
#include<ctime>
using namespace std;
struct Student
{
	string name;
	int age;
	int score;
};
struct Teacher
{
	string name;
	struct Student sArry[5];
};
void inPutInformation(struct Teacher tArry[], int len)
{
	string Name = "ABCDE";
	for (int i = 0; i < len; i++)
	{
		tArry[i].name = "Teacher_";
		tArry[i].name += Name[i];
		for (int j = 0; j < 5; j++)
		{
			tArry[i].sArry[j].name = "Student_";
			tArry[i].sArry[j].name += Name[j];
			int random = rand()% 60 +40;
			tArry[i].sArry[j].score = random;
		}
	}
}
void printInformation(struct Teacher tArry[],int len)
{
	for (int i = 0; i < len; i++)
	{
		cout << "老师的姓名：" << tArry[i].name << endl;
		for (int j = 0; j < 5; j++)
		{
			cout << "\t学生的姓名：" << tArry[i].sArry[j].name << "考试分数：" << tArry[i].sArry[j].score << endl;
		}
	}
}
int main(void)
{	
	srand((unsigned int)time(NULL));
	struct Teacher tArry[3];
	int len = sizeof(tArry) / sizeof(tArry[0]);
	inPutInformation(tArry,len);
	printInformation(tArry,len);
	system("pause");
	return 0;
}
```

创建5个人并按年龄排序

```c++
#include<iostream>
#include<string>
#include<ctime>
using namespace std;
struct Hero
{
	string name;
	int age;
	string categories;
};
int main(void)
{	
	struct Hero heroArry[5] =
	{
		{"欣南",20,"火"},
		{"东杉",24,"木"},
		{"北淼",23,"水"},
		{"坤中",18,"土"},
		{"西昭",22,"金"},
	};
	int len = sizeof(heroArry) / sizeof(heroArry[0]);
	for (int i = 0; i < len - 1; i++)
	{
		for (int j = 0; j < len - 1 - i; j++)
		{
			int temp = 0;
			if (heroArry[j].age > heroArry[j + 1].age)
			{
				temp = heroArry[j].age;
				heroArry[j].age = heroArry[j + 1].age;
				heroArry[j + 1].age = temp;
			}
		}
	}
	for (int i = 0; i <len; i++)
	{
		cout << heroArry[i].name << heroArry[i].age << heroArry[i].categories << endl;
	}
	system("pause");
	return 0;
}
```

## 通讯录

```c++
#include<iostream>
#include<string>
#include<cstdlib>
#define MAX 1000
using namespace std;
struct Person
{
	string name;
	int age;
	string sex;
	string phone;
	string addr;
};
struct addreassbooks
{
	struct Person personarry[MAX];
	int m_Size;
};
void mainMenu()
{
	cout << "--------------------" << endl;
	cout<<"1.增加联系人" << endl;
	cout<<"2.显示联系人" << endl;
	cout<<"3.删除联系人" << endl;
	cout<<"4.查找联系人" << endl;
	cout<<"5.修改联系人" << endl;
	cout<<"6.清空联系人" << endl;
	cout<<"0.退出通讯录" << endl;
	cout << "--------------------" << endl;
}
void addPerson(addreassbooks* abs)
{
	if (abs->m_Size == MAX)
	{	
		string name;
		cout << "联系人已满，无法添加" << endl;
	}
	string name;
	cout << "请输入姓名" << endl;
	cin >> name;
	abs->personarry[abs->m_Size].name = name;
	string sex;
	cout << "请输入性别" << endl;
	cin >> sex;
	abs->personarry[abs->m_Size].sex = sex;
	int age;
	cout << "请输入年龄" << endl;
	cin >> age;
	abs->personarry[abs->m_Size].age = age;
	string phone;
	cout << "请输入电话" << endl;
	cin >> phone;
	abs->personarry[abs->m_Size].phone = phone;
	string addr;
	cout << "请输入地址" << endl;
	cin >> addr;
	abs->personarry[abs->m_Size].addr = addr;
	//更新通讯录人数
	abs->m_Size++;
	cout << "添加成功" << endl;
	system("pause");
	system("cls");
}
void printPerson(addreassbooks* abs)
{
	if (abs->m_Size == 0)
	{
		cout << "当前记录为空" << endl;
	}
	else
	{
		for (int i = 0; i < abs->m_Size; i++)
		{
			cout << "姓名\t" << abs->personarry[i].name << endl;
			cout << "性别\t" << abs->personarry[i].sex << endl;
			cout << "年龄\t" << abs->personarry[i].age << endl;
			cout << "电话\t" << abs->personarry[i].phone << endl;
			cout << "地址\t" << abs->personarry[i].addr << endl;
			cout << "\n";
		}
	}
	system("pause");
	system("cls");
}
int checkPerson(addreassbooks* abs, string name)
{
	for (int i = 0; i < abs->m_Size; i++)
	{
		if (abs->personarry[i].name == name)
		{
			return i;
		}
	}
	return -1;
}
void deletePerson(addreassbooks* abs)
{	
	string dname;
	cout << "请输入你要删除的人名" << endl;
	cin >> dname;
	int ret = checkPerson(abs, dname);
	if (ret == -1)
	{
		cout << "查无此人" << endl;
	}
	else
	{
		for (int i = ret; i < abs->m_Size; i++)
		{
			abs->personarry[i] = abs->personarry[i + 1];
		 }
		abs->m_Size--;
		cout << "删除成功" << endl;
	}
	system("pause");
	system("cls");
}
void findPerson(addreassbooks* abs)
{
	string fname;
	cout << "请输入要查找的联系人姓名" << endl;
	cin >> fname;
	int result = checkPerson(abs, fname);
	if (result == -1)
	{
		cout << "查无此人" << endl;
	}
	else
	{
		cout << "姓名\t" << abs->personarry[result].name << endl;
		cout << "性别\t" << abs->personarry[result].sex << endl;
		cout << "年龄\t" << abs->personarry[result].age << endl;
		cout << "电话\t" << abs->personarry[result].phone << endl;
		cout << "地址\t" << abs->personarry[result].addr << endl;
	}
	system("pause");
	system("cls");
}
void modifyPerson(addreassbooks* abs)
{
	string mname;
	cout << "请输入要修改的联系人姓名" << endl;
	cin >> mname;
	int result = checkPerson(abs, mname);
	if (result == -1)
	{
		cout << "查无此人" << endl;
	}
	else
	{
		string name;
		cout << "请输入姓名" << endl;
		cin >> name;
		abs->personarry[result].name = name;
		string sex;
		cout << "请输入性别" << endl;
		cin >> sex;
		abs->personarry[result].sex = sex;
		int age;
		cout << "请输入年龄" << endl;
		cin >> age;
		abs->personarry[result].age = age;
		string phone;
		cout << "请输入电话" << endl;
		cin >> phone;
		abs->personarry[result].phone = phone;
		string addr;
		cout << "请输入地址" << endl;
		cin >> addr;
		abs->personarry[result].addr = addr;	
		cout << "修改成功" << endl;
	}
	system("pause");
	system("cls");
}
void cleanPerson(addreassbooks*abs)//逻辑清空
{
	abs->m_Size = 0;
	cout << "通讯录清空成功！" << endl;
	system("pause");
	system("cls");
}
int main(void)
{

	//创建通讯录结构体变量
	addreassbooks abs;
	//初始化通讯录中当前人员的个数
	abs.m_Size = 0;

	int select = 0;
	while (1)
	{
		mainMenu();
		cin >> select;
		switch (select)
		{
		case 1://添加联系人
			addPerson(&abs);
			break;
		case 2://显示联系人
			printPerson(&abs);
			break;
		case 3://删除联系人
			deletePerson(&abs);
			break;
		case 4://查找联系人
			findPerson(&abs);
			break;
		case 5://修改联系人
			modifyPerson(&abs);
			break;
		case 6://清空联系人
			cleanPerson(&abs);
			break;
		case 0://退出通讯录
			cout << "欢迎下次使用" << endl;
			system("pause");
			return 0;
		default:
			break;
		}
	}
}
```

