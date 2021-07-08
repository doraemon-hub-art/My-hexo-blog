---
title: C++核心编程部分
date: 2021-07-08 18:33:19
tags: 
  - -C++
  - -笔记
categories:
  - C++
cover: /img/c++.png
---

***

相关视频——[黑马程序员匠心之作|C++教程从0到1入门编程,学习编程不再难_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1et411b73Z?p=146&spm_id_from=pageDriver)(84-146)



我的小站——[半生瓜のblog (doraemon2.xyz)](http://doraemon2.xyz/)



1-83笔记——[如果你准备学习C++,并且有C语言的基础，我希望你能简单的过一遍知识点。_半生瓜のblog-CSDN博客](https://blog.csdn.net/qq_51604330/article/details/117753463?spm=1001.2014.3001.5501)

***

![](/images/晃动的小耗子.gif)

# C++核心编程部分

## 内存分区模型

- 代码区：存放函数的二级制代码，由操作系统进行管理的
- 全局区：存放全局变量和静态变量以及常量
- 栈区：由编译器自动分配释放，存放函数的参数值，局部变量等
- 堆区： 由程序员分配和释放，若程序员不释放，程序结束时由操作系统回收

**内存四区意义**：不同区域存放的数据，赋予不同的声明周期，给我们更大的灵活编程

### 程序运行前

在程序编译后，生成了exe可执行文件，未执行该程序前分为两个区域。

代码区：

- 存放cpu执行的机器指令
- 代码区是共享的，共享的目的是对于频繁被执行的程序，只需要在内存中有一份打码即可
- 代码区是只读的，使其只读的原因是防止程序意外的修改了它的指令

全局区：

- 全局变量和静态变量存放于此
- 全局区还包含了常量区，字符串常量和其他常量也存放于此
- 该区域的数据在程序结束之后由操作系统释放

### 程序运行后

栈区：

- 由编译器自动分配释放，存放函数的参数值，局部变量等。
- 注意事项：不要返回局部变量的地址，栈区开辟的数据由编译器自动释放

堆区：

- 有程序员分配释放，若程序员不释放，程序结束之后有操作系统回收
- 在C++中主要利用new在堆区中开辟内存

```c++
int* p = new int(10);
```

### new运算符

在堆区开辟数据

堆区开辟的数据，由程序员手动开辟，手动释放，释放用delete

**语法**：

```c++
new 数据类型
```

利用new创建的数据，会返回该数据对应类型的指针

```c++
int* arry[] = new int[10];//new int(10)就是创建一个存放整型的空间存10
delete[] arry;释放数组的时候要加[]
```



## 引用

### 基本使用

**作用**:给变量起别名

**语法**：数据类型 &别名 = 原名

```c++
int a = 0;
int &b = a;
//a和b操作的是同一块内存
```

### 注意事项

- 引用必须初始化——告诉它它是谁的别名
- 引用在初始化之后，不可以改变

### 做函数参数

**作用**：函数传参时，可以利用引用让形参修饰实参

**优点**：可以简化指针修改实参（传址）。

```c++
void changeNums(int &a,int& b);//引用传递
int main(void)
{
    int a = 10;
    int b = 20;
    changeNums(int &a,int& b)
    //引用——其实上面的a就是下面a的一个别名
    return 0;
}
```

### 做函数的返回值

**注意**：不要返回局部变量引用

用法：函数调用作为左值

```c++
#include<iostream>
using namespace std;
int& test1()
{
	int a = 10;//栈区
	return a;
}
int& test2()
{
	static int b = 20;//静态变量存放在全局区，全局区的数据在程序结束后系统释放
	return b;
}
int main(void)
{
	int& ret = test1();
	int& ret2 = test2();
	cout << ret2 << endl; 
	//作为左值
	test2() = 1000;//如果函数的返回值是引用，这个函数调用可以作为左值
	cout << ret2 << endl;
	cout << ret << endl;//第一次结果正确是因为编译器做了保留
	cout << ret;//第二次结果错误是因为a的内存已经释放
	return 0;
}
```

### 引用本质

**本质**：引用的本质在c++内部实现是一个指针常量,引用一旦被初始化之后就不能更改。

```c++
void func(int& ref)
{
    ref = 100;//ref是引用，转换为*ref = 100
}
int main(void)
{
    int a  = 10;
	int &ref = a;//自动转化int* const ref = &a;//指针常量是指针指向不可改，也说明为什么引用不可更改
	ref =20;//自动发现ref是引用，自动转换为*ref = 20;
}
```

**结论**：C++推荐使用引用技术，因为语法方便，引用本质是指针常量，但所有的指针操作编译器都棒我们做了。

### 常量引用



**作用**:常量引用主要用来修饰形参，防止误操作

```c++
//常量引用
//使用场景，用来修饰形参，防止误操作
//引用必须引用一块合法的内存空间
const int& ref = 10;
//加上const之后，编译器将代码修改为int temp =10;
//int& ref = temp;
//加入const之后变为只读不可以修改
```

在函数形参列表中，可以加const修饰形参，防止形参改变实参

```c++
void showvaL(const int& ref)
{
    
}
```

## 函数提高

### 函数默认参数

在c++中函数形参列表中的形参是可以有默认值的。

语法：返回值类型 函数名(参数=默认值)

```c++
//函数的哪个参数被声明默认了，下面函数调用的时候就可以少传哪个参数，如果有默认值还传了参数，用的就是函数调用传递的参数
int func(int a,int b =10,int c =23)
{
    return a+b+c;
}
int main(void)
{		
    int ref = func(10);
    return 0;
}
```

注意事项：如果某个位置已经有了默认参数，那么从这个位置往后都要有默认参数

```c++
//从b开始往后一的参数都有默认参数
int fun2(int a,int b= 10;int c =20)
{
    
}
```

如果函数的声明有默认参数，函数的实现就不能有默认参数了。

 声明和实现只能有一个有默认参数。

```c++
int fun3(int a = 10;int b = 20);
int fun3(int a,int b)
{
    
}
```

### 函数占位参数

C++中函数的形参列表里可以有占位参数，用来占位，调用函数的时候必须填补该位置。

语法：返回值类型 函数名 （数据类型）{}，

在现阶段函数的占位参数存在意义不大，但是后面的课程中会用到该技术。

```c++
void func(int a, int)
{

}
int main(void)
{
	func(10,1);//这个1传进去是拿不到的，目前阶段的占位参数我们还用不到，但在后面是会用到的。
	return 0;
}
```

占位参数还可以有默认参数

```c++
void func(int a, int =10)
{

}
int main(void)
{
	func(10);
	return 0;
}
```

### 函数重载

**作用**:函数名可以相同，提高复用性

**函数重载满足条件**

- 同一个作用域下
- 函数名相同
- 函数参数类型不同或者个数不同或者顺序不同

**注意**：函数的返回值不可以作为函数重载的条件

```c++
void func()
{
	cout << "无参数" << endl;
}
void func(int a)
{
	cout << a;
}
int main(void)
{
    根据函数传递参数的不同调用不同的代码
	func();
	func(10);
	return 0;
}
```

### 函数重载的注意事项

- 引用作为函数重载条件
- 函数重载碰到函数默认参数

```c++
#include<iostream>
using namespace std;
void func(int &a)
{
	//int& a =10;不合法
}
void func(const int &a)
{
	//const int& a =10;合法——编译器自动优化
}


int main(void)
{
	func(10);
	return 0;
}
```



```c++
#include<iostream>
using namespace std;
void func(int a ,int b = 10)
{
	
}
void func(int a)
{
	
}


int main(void)
{
	func(10);
	/*当函数重载碰到默认参数
	编译器傻了，不知道该调用哪个了
	——出现二义性
	——写函数重载就不要加默认参数，避免这种情况的出现*/
	return 0;
}
```

## 类和对象

C++面向对象的三大特性为：封装、继承、多态。

C++认为万事万物皆为对象，对象上有其属性和行为

**例如**:

人可以作为对象，属性有姓名、年龄、身高、体重......行为有唱、跳、跑......

车也可以作为对象，属性有轮胎、方向盘、大灯......行为有载人、放音乐、开空调......

具有相同性质的对象，我们可以抽象称为类，人属于人类，车属于车类......

### 封装

####  封装的意义

封装是C++面向对象的三大特征之一

封装的意义：

- 将属性和行为作为一个整体，表现生活中的事物
- 将属性和行为加以权限控制

#### **封装的意义一**

在设计类的时候，属性和行为写在一起，表现事物

**语法**:

```c++
class 类名{访问权限: 属性 / 行为};
```

#### 例子

**示例1**

创建一个圆类，求圆的周长

```c++
#include<iostream>
using namespace std;
double pi = 3.14;
//class 代表设计一个类，类后面紧跟着的就是类名称
class Circle
{
	//访问权限
	//公共权限
public:
	//属性 
	//半径
	int c_r;
	//行为
	//获取圆的周长
	double calculateZC()
	{
		return 2 * pi * c_r;
	}
};
int main(void)
{
	//通过圆类创建具体的圆(对象)
	//实例化——通过一个类创建一个对象的过程
	Circle c1;
	//给圆对象的属性进行赋值
	c1.c_r = 10;
	cout << "圆的周长为" << c1.calculateZC() << endl;

	return 0;
}
```

**示例2**

创建一个学生类

```c++
#include<iostream>
#include<string>
using namespace std;
class Student
{

public:
	string s_Name;
	int s_Id;
	void showStudent()
	{
		cout << "姓名： " << s_Name << "ID：" << s_Id << endl;
	}
	//赋值
	void inputName(string name)
	{
		s_Name = name;
	}
};

int main(void)
{
	Student s1;
	//s1.s_Name = "张三";
	s1.inputName("赵六");
	s1.s_Id = 123456;
	s1.showStudent();
	return 0;
}
```

类中的属性和行为，我们统称为成员

属性-成员属性-成员变量

行为-成员函数-成员方法

#### **封装的意义二**

类在设计时，可以把属性和行为放在不同的权限下，加以控制

访问权限有三种

1. public——公共权限——成员类内可以访问，类外可以访问
2. protected--保护权限——成员类内可以访问，类外不可以访问
3. private——私有权限——成员类内可以访问，类外不可以访问

```c++
#include<iostream>
#include<string>
using namespace std;
class Person
{
public:
	string p_name;
protected:
	string p_car;
private:
	int p_password;
public:
	void funcshow()
	{
		p_name = "张三";
		p_car = "拖拉机";
		p_password = 123456;
	}
};

int main(void)
{
	Person p1;
	p1.p_name = "王五";
	//p1.p_car = "GTR";protected类外无法访问
	//p1.p_password = 123;private类外无法访问
	return 0;
}
```

#### struct和class

在C++中struct和class的唯一区别就是默认的访问权限不同。

**区别**：

- struct默认权限为公共public
- class默认权限为私有private

成员属性设置为私有

**优点1**：将所有成员属性设置为私有，可以自己控制读写权限。

**优点2**：对于写权限，我们可以检测数据的有效性。

**示例**:

```c++
#include<iostream>
#include<string>
using namespace std;
class Person
{
public:
	//设置姓名
	void setName(string name)
	{
		p_name = name;
	}
	//获取姓名
	string getName()
	{
		return p_name;
	}
	//获取年龄
	int getAge()
	{

		return p_age;
	}
	//设置年龄
	void setAge(int age)
	{

		p_age = age;
		if (age < 0 || age >150)
		{
			p_age = 0;
			cout << "什么鬼" << endl;
			return;
		}
	}
	//设置伙伴
	void setLover(string lname)
	{
		lover = lname;
	}
private:
	//姓名 可读可写
	string p_name;
	//年龄 可读可写加个范围
	int p_age;
	//伙伴  只写
	string lover;
};

int main(void)
{
	Person p1;
	p1.setName("张三");
	cout << "姓名:" << p1.getName() << endl;
	p1.setAge(18);
	cout << "年龄:" << p1.getAge() << endl;
	p1.setLover("赵四");
	return 0;
}
```

#### 练习案例

##### (1)设计立方体类

求立方体的面积和体积

分别用全局函数和成员函数判断两个立方体是否相等

```c++
#include<iostream>
using namespace std;
class Cube
{
public:

	 void setl(int l)
	 {
		C_L = l;
	 }
	 int getl()
	 {
		 return C_L;
	 }

	 void setw(int w)
	 {
		 C_W = w;
	 }
	 int getw()
	 {
		 return C_W;
	 }

	 void seth(int h)
	 {
		 C_H= h;
	 }
	 int geth()
	 {
		 return C_H;
	 }
	
	 //表面积
	 int calculateS()
	 {
		 return 2 * C_L * C_W + 2 * C_L * C_H + 2 * C_W * C_H;
	 }
	 //体积
	 int calculateV()
	 {
		 return C_L * C_W * C_H;
	 }
	 //成员函数判断是否相等
	 bool issamebyClass(Cube &c)
	 {																	
		 if (C_H== c.geth() && C_L == c.getl() && C_W == c.getw())
		 {
			 return true;
		 }
		 return false;
	 }
private:
	int C_L;
	int C_W;
	int C_H;
};
//利用全局函数判断相等
bool issame(Cube &c1, Cube &c2)
{
	if (c1.geth() == c2.geth() && c1.getl() == c2.getl() && c1.getw() == c2.getw())
	{
		return true;
	}
	return false;
}
int main(void)
{
	Cube c1;
	c1.seth(10);
	c1.setl(10);
	c1.setw(10);
	cout << c1.calculateS() << endl;
	cout << c1.calculateV() << endl;

	Cube c2;
	c2.seth(10);
	c2.setl(10);
	c2.setw(10);
	//判断是否相等
	bool ret = issame(c1, c2);
	if (ret)
	{
		cout << "c1和c2相等" << endl;
	}
	else
	{
		cout << "c1和c2不相等" << endl;
	}
	//成员函数判断
	bool ret2 = c1.issamebyClass(c2); 
	if (ret2)
	{
		cout << "利用成员函数,c1和c2相等" << endl;
	}
	else
	{
		cout << "利用成员函数,c1和c2不相等" << endl;
	}
	system("pause");
	return 0;
}
```

##### (2)点和圆的关系

设计一个圆类和一个点类判断圆和点的关系。

**在一个类中可以让另一个类作为这个类的成员**

```c++
#include<iostream>
using namespace std;
class Point
{
public:
	void setx(int x)
	{
		c_x = x;
	}
	int getx()
	{
		return c_x;
	}
	void sety(int y)
	{
		c_y = y;
	}
	int gety()
	{
		return c_y;
	}
	//建议将属性设置为私有，对外提供接口
private:
	int c_x;
	int c_y;
};
class Circle
{
public:
	void setr(int r)
	{
		c_R = r;
	}
	int getr()
	{
		return c_R;
	}
	void setcenter(Point center)
	{
		c_center = center;
	}
	Point getcenter()
	{
		return c_center;
	}
private:
	int c_R;
	Point c_center;
};
//判断
void isInCircle(Circle &c,Point &p)
{
		int distance =
		(c.getcenter().getx() - p.getx()) * (c.getcenter().getx() - p.getx()) +
		(c.getcenter().gety() - p.gety()) * (c.getcenter().gety() - p.gety());
		int rdistance = c.getr() * c.getr();
		if (distance == rdistance)
		{
			cout << "点在圆上" << endl;
		}
		else if (distance > rdistance)
		{
			cout << "点在圆外" << endl;
		}
		else
		{
			cout << "点在圆内" << endl;
		}
}
int main(void)
{
	Circle c1;
	c1.setr(10);
	Point center;
	center.setx(10);
	center.sety(10);
	c1.setcenter(center);
	Point p1;
	p1.setx(3);
	p1.sety(4);
	//调用判断
	isInCircle(c1, p1);
	return 0;	
}
```

将一个类拆分成两个文件

**point.h**

```c++
#pragma once
#include<iostream>
using namespace std;
class Point
{
public:
	void setx(int x);
	int getx();
	void sety(int y);
	int gety();
private:
	int c_x;
	int c_y;
};
```

**point.cpp**

```c++
#include"point.h"
//Point::告诉编译器这是Point作用域下面的一个成员函数
void Point::setx(int x)
{
	c_x = x;
}
int Point::getx()
{
	return c_x;
}
void Point::sety(int y)
{
	Point::c_y = y;
}
int Point::gety()
{
	return c_y;
}
```

### 对象的初始化清理

- 在生活中我们所购买的点子产品大多都有恢复出厂设置，在某一天我们不使用的时候清楚自己的数据来保证自己信息的安全。
- C++中的面向对象来源生活，每个对象也会有初识设置以及对象销毁前的清理数据的设置。、

#### 构造函数和析构函数

对象的**初始化和清理**也是两个非常重要的安全问题。

一个对象或者变量没有初识状态，对其使用后的后果是未知的。

同样的使用完一个对象或者变量，没有及时进行清理，也会造成一定的安全问题。

C++利用了**构造函数和析构函数**解决上述问题，这两个函数将会被编译器自动斓用，完成对象初始化和清理工作。对象的初始化和清理工作是编译器强制要我们做的事情，**因此如果我们不提供构造和析构，编译器会提供，但是编译器提供的构造函数和析构函数是空实现**。

- 构造函数：主要作用在于创建对象时为对象的成员属性赋值，构造函数由编译器自动调用，无须手动调用。
- 析构函数：主要作用在于对象销毁前系统自动调用，执行一些清理工作。

##### 构造函数语法

```c++
类名(){}
```

1. 构造函数没有返回值也不写void
2. 函数名称与类名相同
3. 构造函数可以有参数，因此可以发生重载
4. 程序在调用对象的时候会自动调用构造，无须手动调用，而且只会调用一次

```c++
#include<iostream>
using namespace std;
class Person
{
public:
	Person()
	{
        //不写的也会自动创建一个，只不过里面是空的
		cout << "构造函数的调用" << endl;
	}
};
void test01()
{
	Person p;//创建了一个对象但是没有调用这个函数
}
int main(void)
{
	test01();
    system("pause");
	return 0;
}
```

![image-20210617103403844](/images/C++核心部分.assets/image-20210617103403844.png)

##### 析构函数语法

```c++
~类名(){}
```

1. 析构函数没有返回值也不写void
2. 函数名称与类名相同，在名称前加上~
3. 析构函数不可以有参数，因此不可以发生重载
4. 程序在对象销毁前会自动调用析构，无须手动调用，而且只会调用一次

```c++
#include<iostream>
using namespace std;
class Person
{
public:
	Person()
	{
		cout << "构造函数的调用" << endl;
	}
	~Person()
	{
		cout << "析构函数的调用" << endl;
	}
	//构造和析构都是必须有的实现，如果我们自己不提供，编译器会提供一个空实现的构造和析构
};
void test01()
{	
	Person p;//在栈上的数据，test01执行完之后会释放这个对象
}
int main(void)
{
	test01();
	//Person p;在main函数中析构函数也会被调用在按完任意键之后
	system("pause");
	return 0;
}
```

![image-20210617112248509](/images/C++核心部分.assets/image-20210617112248509.png)

#### 构造函数的分类及调用

两种分类方式：

- 按参数分为:有参构造和无参构造
- 按类型分为:普通构造和拷贝构造

三种调用方式：

- 括号法

```c++
Person p;//默认构造函数调用
	/*注意：使用默认构造函数的时候，不要加(),编译器会认为这是一个函数的声明
	例如：Person p1();不会认为在创建对象*/
	Person p2(10);//有参构造函数调用
	Person p3(p2);//拷贝构造函数调用
	cout << "p2的年龄为" << p2.age << endl;
	cout << "p3的年龄为" << p3.age << endl;
```



- 显示法

```c++
Person p1;//无参
	Person p2 = Person(10);//有参
	Person p3 = Person(p2);//拷贝
	//如果把等号右边的式子单独拿出来
	//Person(10)这是一个匿名对象-特点——当前行执行结束后，系统会立即回收掉匿名对象
	//注意：不要利用拷贝函数初始化匿名对象-编译器会认为Person(p3) == Person p3 编译器会认为是对象的声明
	//Person(p3)
```



- 隐式转换法

   ```c++
   Person p4 = 10;//相当与Person p4 = Person(10);
   Person p5 = p4;//拷贝构造
   ```
  
  **全部代码**:
  
  ```c++
  #include<iostream>
  using namespace std;
  class  Person
  {	
  public:
  	//构造函数
  	//构造函数-无参构造-编译器提供的就是无参的
  	Person()
  	{
  		cout << "Person的无参构造函数调用" << endl;
  	}
  	//构造函数-有参构造
  	Person(int a)
  	{
  		//将传入的人身上的所有属性，拷贝到我身上。
  		age = a;
  		cout << "Person的有参构造函数调用" << endl;
  	}
  	~Person()
  	{
  		cout << "Person的析构函数调用" << endl;
  	}
  	//////////////
  	//拷贝构造函数
  	Person(const Person	&p)
  	{
  		age = p.age;
  		cout << "拷贝构造函数调用" << endl; 
  	}
  	int age;
  };
  int main(void)
  {
  	//Person p;//默认构造函数调用
  	///*注意：使用默认构造函数的时候，不要加(),编译器会认为这是一个函数的声明
  	//例如：Person p1();不会认为在创建对象*/
  	//Person p2(10);//有参构造函数调用
  	//Person p3(p2);//拷贝构造函数调用
  	//cout << "p2的年龄为" << p2.age << endl;
  	//cout << "p3的年龄为" << p3.age << endl;
  	
  	//显示法
  	//Person p1;//无参
  	//Person p2 = Person(10);//有参
  	//Person p3 = Person(p2);//拷贝
  	////如果把等号右边的式子单独拿出来
  	////Person(10)这是一个匿名对象-特点——当前行执行结束后，系统会立即回收掉匿名对象
  	////注意：不要利用拷贝函数初始化匿名对象-编译器会认为Person(p3) == Person p3 编译器会认为是对象的声明
  	////Person(p3)
  	
  	//隐式转换法
  	Person p4 = 10;//相当与Person p4 = Person(10);
  	Person p5 = p4;//拷贝构造
  	system("pause");
  	return 0;
  }



#### 拷贝构造函数调用时机

C++中拷贝构造函数调用时机通常有三种情况

- 使用一个已经创建完毕的对象来初始化一个新对象
- 值传递的方式给函数参数传值
- 以值方式返回局部对象

```c++
#include<iostream>
using namespace std;
class Person
{
public:
	Person()
	{
		cout << "Person的默认构造函数调用" << endl;
	}
	Person(int age)
	{
		cout << "Person的有参构造函数调用" << endl;
		m_Age = age;
	}
	Person(const Person& p)
	{
		cout << "Person的拷贝构造函数调用" << endl;
		m_Age = p.m_Age;
	}
	~Person()
	{
		cout << "Person的析构函数调用" << endl;
	}
	int m_Age;	
};
//使用一个已经创建完毕的对象来初始化一个对象
void test01()
{
	Person p1(20);
	Person p2(p1);
	cout << "p2的年龄为" << p2.m_Age << endl;
}
//值传递的方式给函数参数传值
void dowork(Person p)
{
	
}
void test02()
{
	Person p;
	dowork(p);
}
//值方式返回局部对象
Person dowork2()
{
	Person p1;
	cout << (int*)&p1 << endl;
	return p1;
}
void test03()
{
	Person p = dowork2();
	cout << (int*)&p << endl;
}

int main(void)
{
	//test01();
	//test02();
	test03();
	system("pause");
	return 0;
}
```

#### 构造函数的调用规则

默认情况下，C++编译器至少给一个类添加三个函数

1. 默认构造函数(无参、函数体为空)
2. 默认析构函数(无参、函数体为空)
3. 默认拷贝函数构造函数，对属性值拷贝

构造函数调用规则如下:

- 如果用户定义有参构造函数，C++不再提供默认无参构造，但是会提供默认拷贝构造
- 如果用户定义拷贝构造函数，C++不会再提供其他构造函数

```c++
#include<iostream>
using namespace std;
//构造函数的调用规则
//只要创建一个类，c++编译器会默认给每个类都添加至少3个函数
/*默认构造(空实现)
析构函数(空实现)
拷贝函数*/
class Person
{
public:
	Person()
	{
		cout << "Person的默认构造函数调用" << endl;
	}
	Person(int age)
	{
		m_Age = age;
		cout << "Person的有参构造函数调用" << endl;
	}
	Person(const Person& p)
	{
		m_Age = p.m_Age;
		cout << "Person的拷贝构造函数调用" << endl;
	}
	~Person()
	{
		cout << "Person的默认析构函数调用" << endl;
	}
	int m_Age;
	
};
void test()
{
	Person p;
	p.m_Age = 18;
	Person p2(p);
	cout << "p2的年龄为" << p2.m_Age << endl;
}
//当用户创建了有参构造函数，编译器就不再提供默认无参构造函数，但是会提供默认拷贝构造函数
void test02()
{
	
}
int main(void)
{
	test02();
	system("pause");
	return 0;

}
```

**总结**：

用户提供了有参，编译器不会提供无参，但会提供拷贝；

用户提供了拷贝，编译器什么构造函数都不会提供。

#### 深拷贝与浅拷贝

深浅拷贝是面试的一个经典的问题，也是常见的一个坑。

**浅拷贝**：简单的赋值拷贝操作。

**深拷贝**：在堆区中重新申请空间，进行拷贝操作。

***

**浅拷贝带来的问题——内存重复释放**。

```c++
#include<iostream>
using namespace std;
//深拷贝与浅拷贝问题
class Person
{
public:
	Person()
	{
		cout << "Person的默认构造函数调用" << endl;
	}
	Person(int age,int height)
	{
		m_Height = new int(height);
		m_Age = age;
		cout << "Person的有参构造函数调用" << endl;
	}
    Person(const Person& p)
	{
		cout << "Person的拷贝构造函数调用" << endl;
		m_Age = p.m_Age;
		m_Height = p.m_Height;编译器默认实现的就是这行代码
		
	}
	~Person()
	{
		//将堆区开辟的数据进行释放
		if (m_Height !=NULL)
		{
			delete m_Height;
			m_Height = NULL;
		}
		cout << "Person的析构构造函数调用" << endl;
	}
 
	int m_Age;
	int* m_Height;//为什么要用指针——要把身高开辟到堆区
};
void test()
{
	Person p1(18,166);
	cout << p1.m_Age<<"\t" << *p1.m_Height << endl;
	Person p2(p1);
	cout << p2.m_Age<<"\t" <<*p2.m_Height<< endl;
}
int main(void)
{
	test();
	system("pause");
	return 0;

}
```

![image-20210703120328939](/images/C++核心部分.assets/image-20210703120328939.png)

![image-20210703120223398](/images/C++核心部分.assets/image-20210703120223398.png)

**浅拷贝的这个问题需要用深拷贝来解决**

重新在堆区找一块内存来存放他。

**自己实现拷贝构造函数来解决浅拷贝带来的问题**

**解决**：

**深拷贝**——手动创建拷贝构造函数。

```c++
	Person(const Person& p)
	{
		cout << "Person的拷贝构造函数调用" << endl;
		m_Age = p.m_Age;
		//m_Height = p.m_Height;编译器默认实现的就是这行代码
		//深拷贝操作
		m_Height = new int(*p.m_Height);
	}
```

**总结**：

如果有属性在堆区开辟的，一定要自己提供拷贝构造函数，防止浅拷贝带来的问题。

#### 初识化列表

**作用**：

C++提供了初始化列表语法，用来初始化对象。

**语法**：

构造函数()：属性1（值1），属性2（值2）...{}

**示例**：

```c++
#include<iostream>
using namespace std;
class Person
{
public:
	//传统赋值操作
	/*Person(int a, int b, int c)
	{
		m_A = a;
		m_B = b;
		m_C = c;
	}*/
	//初始化列表初始化属性
	Person(int a,int b,int c) :m_A(a), m_B(b), m_C(c)
	{

	}
	int m_A;
	int m_B;
	int m_C;
};
void test()
{
	//Person p(10,20,30);
	Person p(30,20,10);
	cout << p.m_A << endl;
	cout << p.m_B << endl;
	cout << p.m_C << endl;
}
int main(void)
{
	test();
	system("pause");
	return 0;

}
```

#### 类对象作为类成员

C++中类的成员可以是另一个类的对象，我们称该成员为对象成员。

**例如**:

```c++
class A{}
class B
{
    A a;
}
```

B类中有对象A作为成员，A为对象成员。

那么当创建B对时，A与B的构造和析构的顺序是怎么样的？

**A先被构造**

**当其他类的对象作为本类的成员时，构造时先构造其他类的对象，再构造自身。**

析构呢？**与构造函数相反。**

**自身的析构函数先进行，之后其它类再进行。**

```c++
#include<iostream>
#include<string>
using namespace std;
class Phone
{
public:
	Phone(string  p)
	{
		Phonename = p;
		cout << "Phone的构造函数调用" << endl;
	}
	~Phone()
	{
		cout << "Phone的析构函数调用" << endl;
	}
	string Phonename;
};

class Person
{
public:
	//Phone Personphone = pname 隐式转换法
	Person(string name, string pname):Personname(name), Personphone(pname)
	{
		cout << "Person的构造函数调用" << endl;
	}
	~Person()
	{
		cout << "Person的析构函数调用" << endl;
	}
	string Personname;
	Phone Personphone;
};
void test()
{
	Person p("张三", "华为"); 
	cout << p.Personname<< endl;
	cout << p.Personphone.Phonename<< endl;
}
int main(void)
{
	test();
	system("pause");
	return 0;

}
```

#### 静态成员 

静态成员就是在成员变量和成员函数前面加上关键字啊static，称为静态成员。

**静态成员分为**：

- 静态成员变量
  - 所有对象共享同一份数据
  - 在编译阶段分配内存
  - 类内声明，类外初始化
- 静态成员函数
  - 所有成员共享同一个函数
  - 静态成员函数只能访问静态成员变量

```c++
#include<iostream>
using namespace std;
class Person
{
public:
	//静态成员函数
	static void func()
	{
		age = 100;//静态的成员函数可以访问静态的成员变量，不可以访问非静态的成员变量
		//无法区分到底是哪个对象的成员变量
		cout << "static void func调用" << endl;
	}
	static int age;
	//静态成员函数也是有访问权限的
private:
	static void func()
	{

	}
};
void test01()
{
	//两种访问方式
	//通过对象访问
	Person p;
	p.func();
	//通过类名也可以访问
	Person::func();
	//Person::func2();类外访问不到私有的静态成员函数
}
int main(void)
{
	test01();
	system("pause");
	return 0;
}
```

### C++对象模型和this指针

#### 成员变量和成员函数分开存储

在C++中，类内的成员变量和成员函数分开存储，

**只有非静态成员变量才属于类的对象上。**

（只有非静态成员变量的大小算进类的大小中，其他的都不算。）

**空对象的大小是1，为的是区分不同类在内存中的占用位置。**

```c++
#include<iostream>
using namespace std;
//成员变量和成员函数是分开存储的
class Person
{
	int m_A;//非静态成员属于类对象上的。 
	static int m_B;//静态的成员变量不属于类的对象上。
	void func() {}//非静态成员函数不属于类的对象上
	static void func2()}//静态成员函数不属于类的对象上
};
int Person::m_B = 10;
void test01()
{
	Person p;
	//空对象占用内存空间为1
	/*C++编译器给每个空对象也分配一个字节的空间，为的是区分空对象在占内存的位置，
	没一个空对象也应该有一个独一无二的内存地址*/
	cout << sizeof(p) << endl;
}
void test02()
{
	Person p;
	cout << sizeof(p) << endl;
}
int main(void)
{
	test01();
	test02();
	system("pause");
	return 0;
}
```

#### this指针的概念

通过上一个知识点《成员变量和成员函数是分开存储的》我们知道C++中成员变量和成员函数是分开存储的。

每一个非静态成员函数只会诞生一份函数实例，也就是说多个同类型的对象会公用一块代码。

那么问题是：这一块代码是如何区分是哪个对象调用自己的呢？



C++通过提供特殊的对象指针，this指针，解决上述问题。

**this指针指向被调用的成员函数所属的对象**。

**(谁调的，this就指向谁)**

this指针是隐含每个非静态成员函数内的一种指针。

this指针不需要定义，直接使用即可。



this指针的用途

- 当形参和成员变量同名时，可用this指针来区分
- 在类的非静态成员函数中返回对象本身，可使用return *this

**解决名称冲突**

**返回对象本身用*this**

```c++
#include<iostream>
using namespace std;
class Person
{
public:
	Person(int age)
	{
		//this指针指向的是被调函数的成员函数所属的对象
		//这里指向的就是p
		this->age = age;
	}
	//返回本体要用应用的方式进行返回
	//这里返回值如果是Person，就创建了一个新的对象
	Person& PersonAddPerson(Person &p)
	{
		this->age += p.age;
		return *this;
	}
	int age;//注意起名规范也可以解决名字冲突的问题
};
//解决对象冲突
void test()
{
	Person p(18);
	cout << p.age << endl;
}
//返回对象本身用*this
void test01()
{
	Person p1(10);
	Person p2(10);
	p2.PersonAddPerson(p1);//将p1和p2的加在一起
	//多次追加,return *this;
	//链式编程思想
	p2.PersonAddPerson(p1).PersonAddPerson(p1);
	cout << p2.age << endl;
}
int main(void)
{
	test01();
	system("pause");
	return 0;
}
```

#### 空指针返回成员函数

C++中空指针也是可以调用成员函数的，但是也要注意有没有用到this指针，如果用到this指针，需要加以判断来保证代码的健壮性。

```c++
#include<iostream>
using namespace std;
class Person
{
public:
	void ShowClassName()
	{
		cout << "this is Person class" << endl;
	}
	void ShowPersonAge()
	{
	
		//提高健壮性，空的就直接返回，防止代码崩溃
		if (this == NULL)
		{
			return;
		}
		//报错原因是因为传入的指针是NULL——无中生有，用一个空指针访问里面的属性 
		cout << this->m_Age << endl;
	}
	int m_Age;
};
void test()
{
	Person* p = NULL;
	p->ShowClassName();
	p->ShowPersonAge();
}
int main(void)
{
	test();
	system("pause");
	return 0;
}
```

#### const修饰成员函数

**常函数**：

- 成员函数后加const后我们称这个函数为**常函数**
- 常函数不可以修改成员属性
- 成员属性声明时加关键字mutable后，在常函数中依然可以修改

**常对象**：

- 声明对象前const称该对象为常对象。
- 常对象只能调用常函数。

```c++
#include<iostream>
using namespace std;
//常函数
class Person
{
public:
	//this指针的本质是指针常量，指针的指向是不可以修改的
	//就相当于Person *const this;
	//在成员函数后面加const修饰的是this指向，让指针指向的值也不可以修改
	void showPerson() const//加个const就不允许修改了
	{
		this->m_b = 100;
		//this = NULL;tbhis指针是不可以修改指针的指向的
	}
	int m_a;
	mutable int m_b;//加了mutable修饰的特殊变量，即使在常函数,常对象中，也可以修改这个值

	void func()
	{
		m_a = 100;//在普通成员函数中是可以修改的
	}
};
void test()
{
	Person P;
	P.showPerson();
}
//常对象
void test1()
{
	const Person p;//在对象前加const，变为常对象
	//p.m_a = 100;
	p.m_b = 100;
	//常对象只能调用常函数 
	p.showPerson();
	//p.func();常对象不能调用普通成员函数，因为普通成员函数可以修改属性。
	
}
int main(void)
{
	test();
	system("pause");
	return 0;
}
```

### 友元

可客厅就是Public，你的卧室就是Private

客厅所有人都可以进去，但是你的卧室只有和你亲密的人可以进。



 在程序中，有些私有属性也想让类外特殊的一些函数或者类进行访问，就需要用到友元技术。



友元的目的就是让一个函数或者类 访问另一个类中的私有元素。



**友元的关键字——friend**



友元的三种实现

- 全局函数做友元
- 类做友元
- 成员函数做友元

#### 全局函数做友元

**就是将此函数在类的最上面写一个声明，前面加一个friend。**

```c++
#include<iostream>
#include<string>
using namespace std;
class Building 
{
	//goodgay全局函数是Building类的一个好朋友，可以访问你家的卧室(私有成员)
	friend void goodgay(Building* building);
public:
	Building()
	{
		m_SittingRoom = "客厅";
		m_BedRoom = "卧室";
	}
public:
	string m_SittingRoom;
private:
	string m_BedRoom;
};

//全局函数
void goodgay(Building* building)
{
	cout << "好基友全局函数正在访问你的" << building->m_SittingRoom << endl;
	
	cout << "好基友全局函数正在访问你的" << building->m_BedRoom << endl;
}
void test()
{
	Building building;
	goodgay(&building);
}
int main(void)
{
	test();
	system("pause");
	return 0;
}
```

#### 类做友元

**一个类在另一个中friend class xx。**

```c+++
#include<iostream>
#include<string>
using namespace std;
//在前面先声明一下
class Building;

class GoodGay
{
public:
	GoodGay();
public:
	void visit();//参观函数 访问Building中的属性
	Building* building;
};


class Building
{
	//GoodGay是Building类的好朋友，可以访问其私有属性
	friend class GoodGay;
public:
	Building();
public:
	string m_SittingRoom;
private:
	string m_BedRoom;
};
//在类外写成员函数
Building::Building()
{
	m_SittingRoom = "客厅";
	m_BedRoom = "卧室";
}
GoodGay::GoodGay()
{
	//创建一个Building对象
	building = new Building;
}
void GoodGay::visit()
{
	cout << "好基友正在访问你的" << building->m_SittingRoom << endl;
	cout << "好基友正在访问你的" << building->m_BedRoom << endl;
}

void test()
{
	GoodGay gy;
	gy.visit();
}
int main(void)
{
	test();
	system("pause");
	return 0;
}
```

#### 成员函数做友元

**告诉编译器 另一个类中的xx成员函数作为本类的好朋友，可以访问私有函数。**

```c++
#include<iostream>
#include<string>
using namespace std;

class Building;
class GoodGay
{
public:
	GoodGay();
	void visit();//可以访问Building中私有成员
	void visit1();//不可以访问Building中私有成员
	Building* builidng;	
};
class Building
{
	//告诉编译器 GoodGay类中的visit成员函数作为本类的好朋友，可以访问私有函数
	friend void GoodGay::visit();
public:
	Building(); 
public:
	string m_SittingRoom;
private:
	string m_BedRoom;
};

Building::Building()
{
	m_SittingRoom = "客厅";
	m_BedRoom = "卧室";
}

GoodGay::GoodGay()
{

	builidng = new Building;
}
void GoodGay::visit()
{
	cout << "visit正在访问" << builidng->m_SittingRoom << endl;
	cout << "visit正在访问" << builidng->m_BedRoom << endl;
}
void GoodGay::visit1()
{
	cout << "visit1正在访问" << builidng->m_SittingRoom << endl;

}
void test()
{
	GoodGay gg;
	gg.visit();
	gg.visit1();
}
int main(void)
{
	test();
	system("pause");
	return 0;
}
```

### 运算符重载

运算符重载的概念:对已有的运算符重新进行定义，赋予其另一种功能，以适应不同的数据类型。

#### 加号运算符重载

**作用**：实现两个自定义数据类型相加的运算。

例如：两个整型相加编译器知道该怎么进行运算，如果是两个自定义出来的类型，两个Person想加，编译器就不知道该怎么运算了。

```c++
#include<iostream>
#include<string>
using namespace std;
//加号运算符重载

class Person
{
public:
	//1.成员函数重载+
	/*Person operator+(Person& p)
	{
		Person temp;
		temp.m_A = this->m_A + p.m_A;
		temp.m_B = this->m_B + p.m_B;
		return temp;
	}*/
	int m_A;
	int m_B;
};


//2.全局函数重载+
Person operator+(Person& p1, Person& p2)
{
	Person temp;
	temp.m_A = p1.m_A + p2.m_A;
	temp.m_B = p1.m_B + p2.m_B;
	return temp;
}
//函数函数重载版本
Person operator+(Person& p1, int num)
{
	Person temp;
	temp.m_A = p1.m_A + num;
	temp.m_B = p1.m_B + num;
	return temp;
}
void test01()
{
	Person p1;
	p1.m_A = 10;
	p1.m_B = 10;
	Person p2;
	p2.m_A = 10;
	p2.m_B = 10;
	//成员函数重载本质调用
	//Person p3 = p1.operator+(p2);
	//Person p3 = p1 + p2;//可以简化成这种形式
	//全局函数重载的本质调用
	//Person p3 = operator+(p1,p2);
	/*cout << p3.m_A << endl;
	cout << p3.m_B << endl;*/
	//运算符重载也可以发生函数重载
	Person p3 = p1 + 10;
	cout << p3.m_A << endl;
	cout << p3.m_B << endl;
}
int main(void)
{
{
	test01();
	system("pause");
	return 0;
}
```

**总结**：

1. 对于内置的数据类型的表达式的运算符是不可能改变的
2. 不要滥用运算符重载

#### 左移运算符重载

**作用**：可以输出自定义的类型

```c++
#include<iostream>
using namespace std;
class Person
{
	friend ostream& operator<<(ostream& cout, Person& p);
public:
	Person(int a, int b)
	{
		m_A = a;
		m_B = b;
	}
	//利用成员函数重载左移运算符p.operator<<(cout)简化版本p<<cout
	//一般我们不会利用成员函数来重载<<运算符，以为无法实现cout在左边
	/*void operator<<(ostream &cout,Person &p)
	{
		cout << p.m_A << endl;
		cout << p.m_B << endl;
	}*/
private:
	int m_A;
	int m_B;
};
//只能利用全局函数来重载左移运算符
ostream& operator<<(ostream &cout, Person &p) //这样写的本质就是operator<<(cout,p)简化版本就是cout<<p; 
{
	cout << p.m_A << endl;
	cout << p.m_B << endl;
	return cout;
}
void test()
{
	Person p(10,10);
	cout << p << "hello world" << endl;
}
int main(void)
{
	test();
	system("pause");
	return 0;
}
```

**总结**：重载左移运算符配合友元可以实现输出自定义数据类型。

***

这里给出不推荐的类内实现重载左移运算符

```c++
void operator<<(ostream &cout)
	{
		cout << this->m_A;
		cout << this->m_B;
	}

//使用
p<<cout;
```

#### 递增运算符重载

**作用**：通过重载递增运算符，实现自己的整型数据。

```c++
#include<iostream>
using namespace std;
//重载递增运算符
class MyInteger
{
	friend ostream& operator<<(ostream& cout, MyInteger myint);
public:
	MyInteger()
	{
		m_Num = 0;
	}
	//重载++运算符——前置
	//返回引用是为了一直对一个数据进行递增操作
	MyInteger& operator++()
	{
		++m_Num;
		return *this;
	}
	//重载++运算符——后置
	MyInteger operator++(int)//这个int在这里作为占位参数，用来区分前置递增和后置递增
	{
		MyInteger temp = *this;
		m_Num++;
		return temp;
		//后置递增要返回值，因为如果返回引用，这里相当于返回的是一个局部对象的引用。
		//局部对象在当前函数执行完毕之后就被释放掉了，还要返回引用就是非法操作。
	}
private:
	int m_Num;
};
//全局函数重载左移运算符
ostream& operator<<(ostream& cout, MyInteger myint)
{
	cout << myint.m_Num << endl;
	return cout;
 }
void test()
{
	MyInteger myint;
	cout << ++(++myint);
	cout <<myint;
}
void test02()
{
	MyInteger myint;
	cout << myint++ << endl;
	cout << myint << endl;
}
int main(void)
{
	//test();
	test02();
	system("pause");
	return 0;
}
```

**总结**:前置递增返回引用，后置递增返回值。

#### 赋值运算符重载

C++编译器至少给一个类添加4个函数(前三个之前已经讲过了)

1. 默认构造函数(无参，函数体为空)
2. 默认析构函数(无参，函数体为空)
3. 默认拷贝构造函数，对属性进行值拷贝
4. 赋值运算符operator=，对属性进行值拷贝



如果类中有属性指向堆区，做赋值操作时也会出现深浅拷贝问题。

```c++
#include<iostream>
using namespace std;
class Person
{
public:
	Person(int age)
	{
		m_Age = new int(age);
	}
	~Person()
	{
		if (m_Age != NULL)
		{
			delete m_Age;
			m_Age = NULL;
		}
	}
	//重载赋值运算符
	Person& operator=(Person &p)
	{
		//编译器默认提供的是浅拷贝操作
		//m_Age = p.m_Age;
		//应该先判断是否有属性在堆区，如果有先释放干净，然后再深拷贝。
		if (m_Age != NULL)
		{
			delete m_Age;
			m_Age = NULL;
		}
		//深拷贝操作
		m_Age = new int(*p.m_Age);
		return *this;
	}
	int *m_Age;
};
void test1()
{
	Person p1(18);
	Person p2(20);
	Person p3(30);
	p3 = p2 = p1;
	cout << *(p1.m_Age) << endl;
	cout << *(p2.m_Age) << endl;
	cout << *(p3.m_Age) << endl;
}
int main(void)
{
	test1();
	system("pause");
	return 0;
}
```

#### 关系运算符重载

**作用**：重载关系运算符，可以让两个自定义类型对象进行对比操作

```c++
#include<iostream>
#include<string>
using namespace std;
class Person
{
public:
	//重载==
	bool operator==(Person &p)
	{
		if (this->m_Name == p.m_Name && this->m_Age == p.m_Age)
		{
			return true;
		}
		else
		{
			return false;
		}
	}
	bool operator!=(Person &p)
	{
		if (this->m_Name == p.m_Name && this->m_Age == p.m_Age)
		{
			return false;
		}
		else
		{
			return true;
		}
	}
	Person(string name, int age)
	{
		m_Name = name;
		m_Age = age;
	}
	string m_Name;
	int m_Age;
};
void test()
{
	Person p1("张三", 20);
	Person p2("张三", 20);
	if (p1 == p2)
	{
		cout << "p1和p2是相等的" << endl;
	}
	else
	{
		cout << "p1和p2是不相等的" << endl;
	}
	if (p1 != p2)
	{
		cout << "p1和p2是不相等的" << endl;
	}
	else
	{
		cout << "p1和p2是相等的" << endl;
	}
}
int main(void)
{
	test();
	system("pause");
	return 0;
}
```

#### 函数调用运算符重载

- 函数调用运算符()也可以重载
- 由于重载后使用的方式非常像函数的调用，因此称为仿函数
- 仿函数没有固定写法，非常灵活

```c++
#include<iostream>
#include<string>
using namespace std;
//函数调用运算符重载
class MyPrint
{
public:
	//重载函数调用运算符
	void operator()(string text)
	{
		cout << text << endl;
	}
	
};
class MyAdd
{
public:
	int operator()(int a, int b)
	{
		return a + b;
	}
};
void test()
{
	MyPrint myprint;
	myprint("hello world");
	MyAdd myadd;
	cout << myadd(1, 2) << endl;
	//匿名函数对象——特点:当前行被执行完立即释放
	cout << MyAdd()(100,100) << endl;
}
int main(void)
{
	test();
	system("pause");
	return 0;
}
```

### 继承

**继承是面向对象三大特性之一**

有些类与类之间存在特殊的关系，例如下图中:

![image-20210705121808431](/images/C++核心部分.assets/image-20210705121808431.png)

我们发现，定义这些类的时候，下级别的成员除了拥有上一级的共性，还有自己的特性。

**这时候我们就可以考虑利用继承的技术，减少重复代码量。**

#### 继承的基本语法

例如我们看到很多网站中，都有公共的头部，公共的底部，甚至公共的左侧列表，只有中心内容不同。

接下里我们分别利用普通写法和继承写法来实现网页中的内容，看一下继承存在的意义以及好处。

**普通实现**：

```c++
#include<iostream>
#include<string>
using namespace std;

//普通实现页面

//java页面
class Java
{
public:
	void header()
	{
		cout << "首页、登录注册" << endl;
	}
	void footer()
	{
		cout << "帮助中心、交流合作" << endl;
	}
	void left()
	{
		cout << "java、python、c++" << endl;

	}
	void contenet()
	{
		cout << "java学科视频" << endl;
	}
};
class Python
{
public:
	void header()
	{
		cout << "首页、登录注册" << endl;
	}
	void footer()
	{
		cout << "帮助中心、交流合作" << endl;
	}
	void left()
	{
		cout << "java、python、c++" << endl;

	}
	void contenet()
	{
		cout << "python学科视频" << endl;
	}
};
class Cpp
{
public:
	void header()
	{
		cout << "首页、登录注册" << endl;
	}
	void footer()
	{
		cout << "帮助中心、交流合作" << endl;
	}
	void left()
	{
		cout << "java、python、c++" << endl;

	}
	void contenet()
	{
		cout << "c++学科视频" << endl;
	}
};
void test()
{
	cout << "java" << endl;
	Java java;
	java.header();
	java.footer();
	java.left();
	java.contenet();

	cout << endl;

	cout << "python" << endl;
	Python python;
	python.header();
	python.footer();
	python.left();
	python.contenet();

	cout << endl;

	cout << "cpp" << endl;
	Cpp cpp;
	cpp.header();
	cpp.footer();
	cpp.left();
	cpp.contenet();
}
int main(void)
{
	test();
	system("pause");
	return 0;
}
```

**继承方法实现**

```c++
#include<iostream>
#include<string>
using namespace std;

//公共页面
class BasePage
{
public:
	void header()
	{
		cout << "首页、登录注册" << endl;
	}
	void footer()
	{
		cout << "帮助中心、交流合作" << endl;
	}
	void left()
	{
		cout << "java、python、c++" << endl;

	}
};
//普通实现页面

//java页面
class Java : public BasePage
{
public:
	void contenet()
	{
		cout << "java学科视频" << endl;
	}
};
class Python : public BasePage
{
public:
	void contenet()
	{
		cout << "python学科视频" << endl;
	}
};
class Cpp : public BasePage
{
public:
	
	void contenet()
	{
		cout << "c++学科视频" << endl;
	}
};
void test()
{
	cout << "java" << endl;
	Java java;
	java.header();
	java.footer();
	java.left();
	java.contenet();

	cout << endl;

	cout << "python" << endl;
	Python python;
	python.header();
	python.footer();
	python.left();
	python.contenet();

	cout << endl;

	cout << "cpp" << endl;
	Cpp cpp;
	cpp.header();
	cpp.footer();
	cpp.left();
	cpp.contenet();
}
int main(void)
{
	test();
	system("pause");
	return 0;
}
```

**总结**：
**继承的好处**：减少重复代码

**语法**：class 子类:继承方式 父类

子类也称派生类

父类也称基类

**派生类中的成员，包含量大部分**

一类是从基类继承过来的，一类是自己增加的成员。

从基类继承过来的表现其共性，而新增加的成员体现其个性。

#### 继承方式

```c++
继承的语法——class 子类 :继承方式 父类
```

**继承方式一共有三种**：

- 公共继承
- 保护继承
- 私有继承

![image-20210705160731648](/images/C++核心部分.assets/image-20210705160731648.png)

```c++
#include<iostream>
using namespace std;

//公共继承
class Base1
{
public:
	int m_A;
protected:
	int m_B;
private:
	int m_C;
};

class Son1 :public Base1
{
public:
	void func()
	{
		m_A = 10;//父类中的公共权限成员，到了子类中依然是公共权限
		m_B = 20;//父类中的保护权限成员，到了子类中依然是保护权限
		//m_C = 10;父类中的隐私权限成员，子类访问不到
	}
};
void test01()
{
	Son1 son1;
	son1.m_A = 100;
	//son1.m_B = 100;保护权限的内容到了类外就无法访问了
};
//保护继承
class Base2
{
public:
	int m_A;
protected:
	int m_B;
private:
	int m_C;
};
class Son2 :protected Base2
{
	void func()
	{
		m_A = 100;//父类中公共权限的成员，因为是保护继承，到子类中变为保护权限
		m_B = 100;//父类中保护权限的成员，保护继承后到了子类还是保护权限。
		//m_C = 100;父类中的私有成员子类访问不到
	}
};
void test02()
{
	Son2 son2;
	//保护权限类外访问不到，所以在son2中m_A也访问不到了
}
//私有继承
class Base3
{
public:
	int m_A;
protected:
	int m_B;
private:
	int m_C;
};
class Son3:private Base3
{
	void func()
	{
		m_A = 100;//父类中公共成员，私有继承后，到了子类变为私有成员
		m_B = 100;//父类中保护成员，私有继承后，到了子类变为私有成员
		//m_C = 100;父类的私有权限成员仍然访问不到
	}
};
void test03()
{
	Son3 son3;
	//私有成员类外访问不到
}
//验证Son3私有继承后成员是否变成了私有属性
class GrandSon3 :public Son3
{
	void func()
	{
		//访问不到父类的私有成员
		//到了Son3中m_A,m_B,m_C全是私有成员，子类无法访问
	}
};
int main(void)
{

	system("pause");
	return 0;
}
```

#### 继承中的对象模型

**问题**：从父类继承过来的对象，哪些属于子类对象？

**父类中所有的非静态成员属性都会被子类继承下去**。

**父类中私有的成员属性是被编译器给隐藏了，因此访问不到，但是确实被继承下去了**

```c++
#include<iostream>
using namespace std;
//继承中的对象模型
class Base
{
public:
	int m_A;
protected:
	int m_B;
private:
	int m_C;
};
class Son:public Base 
{
public:
	int m_D;
};
void test01()
{
	//父类中所有的非静态成员属性都会被子类继承下去
	//父类中私有的成员属性是被编译器给隐藏了，因此访问不到，但是确实被继承下去了
	cout << "sizeof of son:" << sizeof(Son) << endl;//结果是16 = 12 + 4
}
int main(void)
{
	test01();
	system("pause");
	return 0;
}
```

**利用VS的开发人员命令提示工具查看对象模型**

![image-20210705170221524](/images/C++核心部分.assets/image-20210705170221524.png)

1. 打开工具
2. 跳转到你cpp文件所在的盘
3. cd文件目录下
4. 输入命令：cd /d1 reportSingleClassLayout类名 文件名

![image-20210705170115915](/images/C++核心部分.assets/image-20210705170115915.png)

#### 继承中构造和析构的顺序

**子类继承父类后，当创建子类时，也会调用父类的构造函数。**

问题：父类和子类的构造函数和析构顺序怎么样的呢？

**先构造父类，再构造子类**

**先析构子类，再析构父类**

**创建子类对象的同时也会创建一个父类对象**。

```c++
#include<iostream>
using namespace std;
class Base
{
public:
	Base()
	{
		cout << "父类的构造函数" << endl;
	}
	~Base()
	{
		cout << "父类的析构函数" << endl;
	}
};
class Son:public Base 
{
public:
	Son()
	{
		cout << "子类的构造函数" << endl;
	}
	~Son()
	{
		cout << "子类的析构函数" << endl;
	}
};
void test01()
{
	Son son;
}
int main(void)
{
	test01();
	system("pause");
	return 0;
}
```

![image-20210705171401072](/images/C++核心部分.assets/image-20210705171401072.png)

**总结**：继承中先调用父类构造函数，再调用子类构造函数，析构顺序与构造相反。

#### 继承同名成员处理方式

问题：当子类与父类出现同名的成员。如何通过子类对象，访问到子类或父类中同名的数据呢?

- 访问子类同名成员，直接访问即可
- 访问父类同名成员，需要加**作用域**

```c++
#include<iostream>
using namespace std;
class Base
{
public:
	Base()
	{
		m_A = 100;
	}
	void func()
	{
		cout << "父类同名成员函数调用" << endl;
	}
	void func(int a)
	{
		cout << "父类同名重载成员函数调用" << endl;
	}
	int m_A;
};
class Son:public Base 
{
public:
	Son()
	{
		m_A = 200;
	}
	void func()
	{
		cout << "子类同名成员函数调用" << endl;
	}
	int m_A;
};
//同名成员属性处理方式
void test01()
{
	Son son;
	cout <<son.m_A<< endl;
	//如果要通过子类对象访问到父类中的同名成员，需要加作用域。
	cout <<son.Base::m_A<< endl;
}
//同名成员函数处理方式
void test02()
{
	Son son1; 
	son1.func();//子
	son1.Base::func();//父
	//如果子类中出现和父类同名的成员函数
	//子类的同名成员会隐藏掉父类中所有同名成员函数
	//如果想要访问到父类中被隐藏的同名成员函数，需要加作用域
	son1.Base::func(10);
}
int main(void)
{
	test02();
	system("pause");
	return 0;
}
```

**总结**：

1. 子类对象可以直接访问到子类中同名成员
2. 子类对象加作用域可以访问到父类同名成员
3. 当子类与父类拥有同名的成员函数，子类会隐藏父类中同名成员函数，加作用域可以访问到父类同名函数。

#### 继承同名静态成员处理方式

问题:继承中同名的静态成员在子类对象上是如何进行访问的呢？



静态成员和非静态成员出现同名，处理方式 一致。



- 访问子类同名成员，直接访问即可
- 访问父类同名成员，需要加作用域

```c++
#include<iostream>
using namespace std;
class Base
{
public:
	static void func()
	{
		cout << "父类静态成员函数调用" << endl;
	}
	static void func(int a)
	{
		cout << "父类静态成员重载函数调用" << endl;
	}
	static int m_A;
};
int Base::m_A = 100;
class Son :public Base
{
public:
	static void func()
	{
		cout << "子类静态成员函数调用" << endl;
	}
	static int m_A;
};
int Son::m_A = 200;
//同名静态成员
void test()
{
	//通过对象访问
	Son son1;
	cout << "通过对象访问" << endl;
	cout << son1.m_A << endl;
	cout << son1.Base::m_A << endl;
	//通过类名访问
	cout << "通过类名访问" << endl;
	cout << Son::m_A << endl;
	//第一个::代表通过类名方式访问，第二个::代表访问父类作用域下
	cout << Son::Base::m_A << endl;
}
//同名静态函数
void test01()
{
	//通过对象访问
	Son son2;
	cout << "通过对象访问" << endl;
	son2.func();
	son2.Base::func();  
	//通过类名访问
	cout << "通过类名访问" << endl;
	Son::func();
	Son::Base::func();

	//父类同名重载成员函数调用
	//子类出现和父类同名的静态成员函数，也会隐藏掉父类中所有同名成员函数(重载)
	//如果想访问父类中被隐藏的同名成员，需要加作用域
	Son::Base::func(100);
}
int main(void)
{
	test();
	cout << "我是分割线------" << endl;
	test01();
	system("pause");
	return 0;
}
```

**总结**:同名静态成员处理方式和非静态处理方式一样，只不过有两种访问的方式(通过对象和类名)。

#### 多继承语法

C++允许一个类继承多个类	

语法:

```c++
class 子类:继承方式 父类1，继承方式 父类2
```

多继承可能会引发父类中有同名成员出现，需要加作用域区分



**C++实际开发中不建议使用多继承**

```c++
 #include<iostream>
using namespace std;
//多继承语法
class Base1
{
public:
	Base1()
	{
		m_A = 100;
	}
	int m_A;
};
class Base2
{
public:
	Base2()
	{
		m_A = 200;
	}
	int m_A;
};
//子类需要继承base1和base2
class Son:public Base1,public Base2
{
public:
	Son()
	{
		m_C = 300;
		m_D = 400;
	}
	int m_C;
	int m_D;
};
void test01()
{	
	Son son1;
	cout << sizeof(son1) << endl;//16
	cout << "第一个父类的m_A:" << son1.Base1::m_A<<endl;
	cout << "第二个父类的m_A:" << son1.Base2::m_A<<endl;
}
int main(void)
{
	test01();
	system("pause");
	return 0;
}
```

**总结**：多继承中如果父类中出现了同名情况，子类使用时要加作用域。

#### 菱形继承

**菱形继承概念**：

两个派生类继承同一个基类，又有某个类同时继承这两个派生类，这种继承称为菱形继承，或者钻石继承。

**典型的菱形继承案例**

![image-20210706120657382](/images/C++核心部分.assets/image-20210706120657382.png)

**菱形继承问题**：

1. 羊继承了动物的数据，驼同样继承了动物的数据，当草泥马使用数据时，就会产生二义性。
2.  草泥马继承动物的数据继承了两份，其实我们应该清楚，这份数据我们只需要一份就可以。

vbptr——虚基类

继承了两个指针，两个指针通过偏移量找到了唯一的数据。

![image-20210706124701407](/images/C++核心部分.assets/image-20210706124701407.png)

```c++
#include<iostream>
using namespace std;
class Animal
{
public:
	int m_Age;
};
//利用虚继承可以解决菱形继承问题
//在继承之前加上关键字virtual变为虚继承
// Animal类称为虚基类
//羊
class Sheep:virtual public Animal
{
		
};
//驼
class Tuo:virtual public Animal
{

};
//羊驼
class SheepTuo :public Sheep,public Tuo
{

};
void test01()
{
	SheepTuo st;
	st.Sheep::m_Age = 18;
	st.Tuo::m_Age = 28;
	//当菱形继承，当两个父类拥有相同的数据，需要加作用域来区分
	cout << st.Sheep::m_Age << endl;
	cout << st.Tuo::m_Age << endl;
	cout << st.m_Age << endl;
	//这份数据我们知道，只有一份就可以了，菱形继承导致数据有两份，资源浪费
}
int main(void)
{
	test01();
	system("pause");
	return 0;
}
```

**总结**：



1. 菱形继承带来的主要问题是子类继承两份相同的数据，导致资源浪费以及毫无意义。
2. 利用虚继承可以解决菱形继承问题——**virtual**

### 多态

**多态是C++面向对象三大特性之一**

#### 多条的基本概念

多态分为两种

- 静态多态:函数重载和运算符重载属于静态多态，复用函数名
- 动态多态:派生类和虚函数实现运行时多态

静态多态和动态多态的区别

- 静态多态的函数地址早绑定 -  编译阶段确定函数地址
- 动态多态的函数地址晚绑定 - 运行阶段确定函数地址

```c++
#include<iostream>
using namespace std;
class Animal
{
public:
	//加上virtual变成虚函数,实现地址晚绑定
	virtual void speak()
	{
		cout << "动物在说话"<< endl;
	}
};

class Cat :public Animal
{
public:
	void speak()
	{
		cout << "小猫在说话" << endl;
	}
};

class Dog : public Animal
{
public:
	void speak()
	{
		cout << "小狗在说话" << endl;
	}
};
//执行说话的函数
//地址早绑定，在编译阶段就确定函数地址
//如果想让猫说话，那么这个函数的地址就不能提前绑定，需要在运行阶段进行绑定

//动态多条满足条件
/*
1.有继承关系
2.子类重写父类的虚函数
*/
//重写要求:函数返回值类型 函数名 参数列表 完全相同 
//动态多态的使用
/*
父类的指针或者引用 指向子类的对象//Animal &animal = cat;
*/

void doSpeak(Animal &animal)//Animal &animal = cat;
{
	animal.speak();
}
void test01()
{
	Cat cat;
	doSpeak(cat);
	Dog dog;
	doSpeak(dog);
}
int main(void)
{
	test01();
	system("pause");
	return 0;
}
```

**总结**：

多态满足条件

- 有继承关关系
- 子类重写父类中的虚函数

多态的使用条件

- 父类指针或引用指向子类对象

重写:函数返回值类型 函数名 参数列表 完全一致称为重写

#### 多态的原理剖析

**虚函数(表)指针**

```c++
vfptr
    v - virtual
    f - functio n
    prt - pointer
```

**虚函数表**

表内记录一个虚函数的地址

```c++
vftable
    v - virtual
    f - functio n
```

当子类重写父类的虚函数后，子类中的虚函数表内部会替换成子类的虚函数地址。

![image-20210706150050385](/images/C++核心部分.assets/image-20210706150050385.png)



![image-20210706150436954](/images/C++核心部分.assets/image-20210706150436954.png)

Cat子类重写前

![image-20210706150747256](/images/C++核心部分.assets/image-20210706150747256.png)

重写后

![image-20210706150753020](/images/C++核心部分.assets/image-20210706150753020.png)

#### 多态案例1——计算器类

案例描述:
分别利用普通写法和多态技术，设计实现两个操作数进行运算的计算器类。



**多态的优点**：

- 代码组织结构清晰
- 可读性强
- 利于前期和后期的扩展以及维护

**代码实现**:

普通方法

```c++
#include<iostream>
#include<string>
using namespace std;
class Calculator
{
public:
	int getResult(string oper)
	{
		if (oper == "+")
		{
			return m_Num1 + m_Num2;
		}
		else if (oper == "-")
		{
			return m_Num1 - m_Num2;
		}
		else if (oper == "*")
		{
			return m_Num1 * m_Num2;
		}
		//如果想扩展新的功能，需要修改原码
		//在真实的开发中，实行开闭原则，对扩展进行开放，对修改进行关闭
	}
	int m_Num1;
	int m_Num2;
};
void test()
{
	Calculator c;
	c.m_Num1 = 10;
	c.m_Num2 = 10;
	cout << c.m_Num1 << "+" << c.m_Num2 << "=" << c.getResult("+") << endl;
}
int main(void)
{
	test();
	system("pause");
	return 0;
}
```

多态方法

```c++
#include<iostream>
#include<string>
using namespace std;

//利用多态实现计算器
//实现计算器抽象类
class AbstractCalculator
{
public:
	virtual int getResult()
	{

		return 0;
	}
	int m_Num1;
	int m_Num2;
};
//加法计算器类
class AddCalculator :public AbstractCalculator
{
public:
	int getResult()
	{
		return m_Num1 + m_Num2;
	}
};
//减法计算器类
class SubCalculator :public AbstractCalculator
{
public:
	int getResult()
	{
		return m_Num1 - m_Num2;
	}
};
//乘法计算器类
class MulCalculator :public AbstractCalculator
{
public:
	int getResult()
	{
		return m_Num1 * m_Num2;
	}
};
void test()
{
	//多态使用条件
	//父类指针或者引用指向子类对象
	//加法
	AbstractCalculator* abc = new AddCalculator;//父类指针指向子类对象
	abc->m_Num1 = 10;
	abc->m_Num2 = 10;
	cout << abc->m_Num1 << "+" << abc->m_Num2 << "=" << abc->getResult() << endl;
	//堆区数据，手动开辟手动释放
	delete abc;//堆区的数据被销毁了，但是指针的类型没有变
	// 减法
	abc = new SubCalculator;
	abc->m_Num1 = 10;
	abc->m_Num2 = 10;
	cout << abc->m_Num1 << "-" << abc->m_Num2 << "=" << abc->getResult() << endl;
	delete abc;
}
int main(void)
{
	test();
	system("pause");
	return 0;
}
```

多态带来的好处

1. 组织结构清晰，哪出错了马上定位到。
2. 可读性强
3. 对于前期和后期扩展以及维护性高

**总结**：C++开发提倡利用多态设计程序框架，因为多态优点很多。

#### 纯虚函数和抽象类

在多态中，通常父类汇中虚函数的实现是毫无意义的，主要都是调用子类重写的内容。

因此可以将虚函数改为纯虚函数。

```c++
纯虚函数语法virtual 返回值类型 函数名 (参数列表) = 0;
```

当类中有了纯虚函数，这个类也称为抽象类。

**抽象类特点**:

- 无法实例化对象
- 子类必须重写抽象类中的纯虚函数，否则也属于抽象类

```c++
#include<iostream>
using namespace std;
//纯虚函数和抽象类
class Base
{
public:
	//只要有一个纯虚函数,这个类称为抽象类
	//特点;无法实例化对象
	virtual void func() = 0;//注意:不要忘掉virtual!
	//抽象类的子类必须要重写父类中的纯虚函数，否则也属于抽象类
};
class Son :public Base
{
public:
	void func()
	{
		cout << "func函数调用" << endl;
	}
};
void test()
{
	//Base b1; 抽象类无法实例化对象
	Son s1;//子类重写父类的虚函数，否则无法实例化对象
	Base* abc = new Son;
	abc->func();
}
int main(void)
{
	test();
	system("pause");
	return 0;
}
```

#### 多态案例2——制作饮品

案例描述:制作饮品的大致流程为:煮水-冲泡-倒入杯中-加入辅料

利用多态技术实现本案例，提供抽象制作饮品基类，提供子类制作咖啡和茶水。

```c++
 #include<iostream>
using namespace std;
//多态案例-制作饮品
class AbstractDrinking
{
public:
	//煮水
	virtual void Boil() = 0;
	//冲泡 
	virtual void Brew() = 0;
	//倒入杯中
	virtual void Pour() = 0;	
	//加入辅料
	virtual void PutSomething() = 0;
	//制作饮品
	void makeDrink()
	{
		Boil();
		Brew();
		Pour();
		PutSomething();
	}
};
//制作咖啡
class Coffee :public AbstractDrinking
{
public:
	//煮水
	virtual void Boil()
	{
		cout << "把水煮开" << endl;
	}
	//冲泡 
	virtual void Brew()
	{
		cout << "冲泡咖啡" << endl;
	}
	//倒入杯中
	virtual void Pour()
	{
		cout << "倒入杯中" << endl;
	}
	//加入辅料
	virtual void PutSomething()
	{
		cout << "加入糖和牛奶" << endl;
	}
};
//制作茶水
class Tea :public AbstractDrinking
{
public:
	//煮水
	virtual void Boil()
	{
		cout << "把矿泉水煮开" << endl;
	}
	//冲泡 
	virtual void Brew()
	{
		cout << "冲泡茶叶" << endl;
	}
	//倒入杯中
	virtual void Pour()
	{
		cout << "倒入杯中" << endl;
	}
	//加入辅料
	virtual void PutSomething()
	{ 
		cout << "加入柠檬" << endl;
	}
};
//制作函数
void DoWork(AbstractDrinking* abs)//父类指针指向子类对象AbstractDrinking* abs = new Coffee;
{
	abs->makeDrink();
	delete abs;//手动释放
	//堆区的数据被销毁了但是指针的类型没变
}
//制作
void test()
{
	DoWork(new Coffee);
	cout << "------我是分割线------" << endl;
	DoWork(new Tea);
}
int main(void)
{
	test();
	system("pause");
	return 0;
}
```

####  虚析构和纯虚析构

多态使用的时候，如果子类中有属性开辟到堆区，那么父类指针在释放的时无法调用到子类的析构代码

**解决方法**:将父类中的析构函数改为虚析构或者纯虚析构

虚析构和纯析构共性:

- 可以解决父类指针释放子类对象，
- 都需要有具体的含函数实现

虚析构和纯虚构的区别：

- 如果是纯虚析构，该类属于抽象类，无法实例化对象

**虚析构语法**;

```c++
virtual ~类名(){}
```

**纯虚析构语法**：

```c++
virtual ~类名() = 0;//声明
```

```c++
类名::~类名(){}
```

```c++
#include<iostream>
#include<string>
using namespace std;
//虚析构和纯虚析构
class Animal
{
public:
	Animal()
	{
		cout << "Animal的构造函数调用" << endl;
	}
	//利用虚析构可以解决父类指针释放对象时不干净的问题
	/*virtual ~Animal()
	{
		cout << "Animal的析构函数调用" << endl;
	}*/
	//纯虚析构,需要声明也需要实现
	//有了纯虚析构之后，这个类也属于抽象类，无法实例化对象	
	virtual ~Animal() = 0;
	//纯虚函数，不需要实现
	virtual void speak() = 0;
};
//纯虚析构函数
Animal::~Animal()
{
	cout << "Animal纯析构函数调用" << endl;
}
class Cat :public Animal
{
public:
	Cat(string name)
	{
		m_Name = new string(name);
	}
	virtual void speak()
	{
		cout << "Cat的构造函数调用" << endl;
		cout << *m_Name << "小猫在说话" << endl;
	}
	~Cat()
	{
		if (m_Name != NULL)
		{
			cout << "Cat的析构函数调用" << endl;
			delete m_Name;
			m_Name = NULL;
		}
	}
	string* m_Name;
};
void test01()
{
	Animal* animal = new Cat("Tom");
	animal->speak();
	/*
	父类的指针在析构的时候，不会调用子类中的析构函数，
	导致子类如果有堆区属性，会出现内存的泄漏情况。
	解决:将父类的析构函数改为虚析构
	*/
	delete animal;
}
int main(void)
{
	test01();
	system("pause");
	return 0;
}
```

**总结**:

1. 虚析构或纯虚析构就是用来解决通过父类指针释放子类对象问题
2. 如果子类中没有堆区数据，可以不写为虚析构或纯虚析构
3. 拥有纯虚析构函数的类也属于抽象类

#### 多态案例3——电脑组装

案例描述:
电脑主要组成部件为CPU(用于计算)，显卡(用于显示)，内存条（用于存储),将每个零件封装出抽象基类，并且提供不同的厂商生产不同的零件，例如Intel厂商和Lenovo厂商创建电脑类提供让电脑工作的函数，并且调用每个零件工作的接口,测试时组装三台不同的电脑进行工作.

```c++
#include<iostream>
using namespace std;
//抽象不同零件类
//抽象cpu
class CPU
{
public:
	//抽象的计算函数
	virtual void calculate() = 0;
};
//抽象显卡类
class VideoCard
{
public:
	//抽象的显示函数
	virtual void display() = 0;
};
//抽象内存条类
class Memory
{
public:
	//抽象的存储函数
	virtual void storage() = 0;
};
//电脑类
class Computer
{
public:
	Computer(CPU* cpu, VideoCard* vc, Memory* mem)
	{
		m_cpu = cpu;
		m_vc = vc;
		m_mem = mem;
	}
	//提供一个工作的函数
	void work()
	{
		//让零件工作起来，调用他的接口
		m_cpu->calculate();
		m_vc->display();
		m_mem->storage();
	}
	//提供析构函数释放3个电脑零件
	~Computer()
	{
		//释放CPU零件
		if (m_cpu != NULL)
		{
			delete m_cpu;
			m_cpu = NULL;
		}
		//释放显卡零件
		if (m_vc != NULL)
		{
			delete m_vc;
			m_vc = NULL;
		}
		//释放内存条零件指针
		if (m_mem != NULL)
		{
			delete m_mem;
			m_mem = NULL;
		}
	}
private:
	CPU* m_cpu;//CPU零件指针
	VideoCard* m_vc;//显卡零件指针
	Memory* m_mem;//内存条零件指针
};
//具体的厂商
//Intel
class IntelCPU :public CPU
{
public:
	virtual	 void calculate()
	{
		cout<<"Intel的CPU开始计算了"<<endl;
	}
};
class IntelVideoCard :public VideoCard
{
public:
	virtual	 void display ()
	{
		cout << "Intel的显卡开始显示了" << endl;
	}
};
class IntelMemory :public Memory
{
public:
	virtual	 void storage()
	{
		cout << "Intel的内存条开始存储了" << endl;
	}
};
//具体的厂商
//Lenovo
class LenovoCPU :public CPU
{
public:
	virtual	 void calculate()
	{
		cout << "Lenovo的CPU开始计算了" << endl;
	}
};
class LenovoVideoCard :public VideoCard
{
public:
	virtual	 void display()
	{
		cout << "Lenovo的显卡开始显示了" << endl;
	}
};
class LenovoMemory :public Memory
{
public:
	virtual	 void storage()
	{
		cout << "Lenovo的内存条开始存储了" << endl;
	}
};
//组装电脑
void test01()
{
	//一台电脑零件
	CPU* intelcpu = new IntelCPU;
	VideoCard* videocard = new IntelVideoCard;
	Memory* memory = new IntelMemory;
	//创建第一台电脑
	Computer* computer1 = new Computer(intelcpu, videocard, memory);
	computer1->work();
	delete computer1;
	cout << "------我是分割线------" << endl;
	//组装第二台电脑
	Computer* computer2 = new Computer(new LenovoCPU, new LenovoVideoCard, new LenovoMemory);
	computer2->work();
	delete computer2;
	cout << "------我是分割线------" << endl;
	//组装第三台电脑
	Computer* computer3 = new Computer(new LenovoCPU,new IntelVideoCard,new LenovoMemory);
	computer3->work();
	delete computer3;
}
int main(void)
{
	test01();
	system("pause");
	return 0;
}
```

## 文件操作

程序运行时，产生的数据都属于临时数据，程序一旦运行结束就会被释放。

通过文件可以将数据持久化。

C++中对文件进行操作需要包含头文件< Fstream>

**文件类型分为两种**:

1. **文本文件**-文件以文本的ASCII码形式存储在计算机中
2. **二进制文件**-文件以文本的二进制形式存储在计算机中，用户一般不能直接读懂他们

**操作文件的三大类**

1. ofstream:写操作
2. ifstream：读操作
3. fstream:读写操作

#### 文本文件

##### 写文件

1. 包含头文件——#include< fstream>
2. 创建流对象——ofstream ofs;
3. 打开文件——ofs.open("文件路径",打开方式)
4. 写数据——ofs<<"写入的数据";
5. 关闭文件——ofs.close();

 文件打开方式:

![image-20210708171215528](/images/C++核心部分.assets/image-20210708171215528.png)

**注意**:文件打开方式可以配合使用，利用|操作符

**例如**:用二进制方式写文件

```c++
ios::binary | ios::out
```

```c++
#include<iostream>
#include<fstream>
using namespace std;
//文本文件写文件
void test01()
{
	//1.包含头文件
	//2.创建流对象
	ofstream ofs;
	//3.指定打开方式
	ofs.open("test.txt", ios::out);//如果不指定文件路径，默认和你项目的文件路径一样
	//4.写内容
	ofs << "姓名:张三" << endl;
	ofs << "性别:男" << endl;
	ofs << "年龄:18" << endl;
	//5.关闭文件
	ofs.close();
}
int main(void)
{
	test01();
	system("pause");
	return 0;
}
```

**总结**:

- 文件操作必须包含头文件fstream
- 读文件可以利用ofstream,或者fstream类
- 打开文件时候需要指定操作文件的路径，以及打开方式
- 利用<<可以向文件中写数据
- 操作完毕，要关闭文件

##### 读文件

读文件操作与写文件步骤相似，但是读取方式比较多

读文件操作步骤如下

1. 包含头文件——#include< fstream>
2. 创建流对象——ifstream ifs;
3. 打开文件并判断文件是否打开成功——ifs.open("文件路径"，打开方式);
4. 读数据——四种方式读取
5. 关闭文件——ifs.close();

```c++
#include<iostream>
#include<fstream>
#include<string>
using namespace std;
void test01()
{
	//1.包含头文件
	//2.创建流对象
	ifstream ifs;
	//3.打开文件,并且判断是否打开成功
	ifs.open("test.txt",ios::in);
	if (!ifs.is_open())
	{
		cout << "文件打开失败了" << endl;
		return;
	}
	//4.读数据
	//第一种
	/*char buf[1024] = { 0 };
	while (ifs>>buf)
	{
		cout << buf << endl;
	}*/
	//第二种
	/*char buf[1024] = { 0 };
	while (ifs.getline(buf,sizeof(buf)))
	{
		cout << buf << endl;
	}*/
	//第三种
	/*string buf;
	while (getline(ifs,buf))
	{
		cout << buf << endl;
	}*/
	//第四种-不推荐
	char c;
	while ((c = ifs.get()) != EOF)//EOF——end of file
	{
		cout << c;
	}
	ifs.close();
}
int main(void)
{
	test01();
	system("pause");
	return 0;
}
```

**总结**

- 读文件可以利用ifsteam,或者fstream类
- 利用is_open函数可以判断是否打开成功
- close关闭文件

#### 二进制文件

以二进制的方式对文件进行读写操作

打开方式主要为**ios::binary**

##### 写文件

二进制方式写文件主要利用流对象调用成员函数write

函数原型:

```c++
ostream& wirte(const char* buffer,int len);
```

参数解释：字符指针buffer指向内存中一段存储空间。len是读写的字节数

```c++
#include<iostream>
#include<fstream>
using namespace std;
//二进制写文件
class Person
{
public:
	char m_Name[64];
	int m_Age;
};
void test01()
{
	//1.包含头文件
	//2.创建头文件
	ofstream ofs("person.txt", ios::out | ios::binary);
	//3.打开文件
	//ofs.open("person.txt",ios::out | ios::binary);
	//4.写文件
	Person p = { "张三",18 };
	ofs.write((const char*)&p,sizeof(Person));
	//5.关闭文件
	ofs.close();
}
int main(void)
{
	test01();
	system("pause");
	return 0;
}
```

**总结**:

- 文件输出流对象，可以通过write函数，以二进制的方式写数据

##### 读文件

二进制方式读文件主要利用流对象调用成员函数read

函数原型:

```c++
istream& read(char * buffer,int len);
```

参数解释:字符指针buffer指向内存中一段存储空间。len是读写的字节数

```c++
#include<iostream>
#include<fstream>
using namespace std;
//二进制读文件
class Person
{
public:
	char m_Name[64];
	int m_Age;
};
void test01()
{
	//1.包含头文件
	//2.创建流对象
	ifstream ifs;
	//3.打开文件&判读文件是否打开成功
	ifs.open("person.txt", ios::in | ios::binary);
	if (!(ifs.is_open()))
	{
		cout<<"打开失败"<<endl;
		return;
	}
	//4.读文件
	Person p;
	ifs.read((char*)&p, sizeof(Person));
	cout << "姓名:" << p.m_Name<<" " << "年龄:" << p.m_Age << endl;
	//5.关闭文件
	ifs.close();
}
int main(void)
{
	test01();
	system("pause");
	return 0;
}
```

**总结**:

文件输入流对象，可以通过read函数，以二进制的方式读数据。

