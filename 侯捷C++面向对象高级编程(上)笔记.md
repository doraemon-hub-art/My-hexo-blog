---
title: 侯捷C++面向对象高级编程(上)笔记
date: 2021-09-09 15:47:43
tags:
categories:
cover: /img/c++.png
---

# 前言

面向对象，就是将数据和处理这些数据的函数包在一起。数据只有这个函数能够看到，不会和其他的混杂在一起。

```c++
c++ class ->  c struct + 更多的特性
```

C++的结构几乎等同于class。

# C++ Programs代码基本形式

## 文件类型

```c++
.h(header files)
.cpp 
```

延伸文件名(extension file name)不一定是.h Or .cpp,也可能是.hpp或者其他甚至无扩展名。

## 头文件写法

**防止头文件重复包含**

![image-20210906123808403](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210906123808403.png)

```c++
#ifndef _XXX
#define _XXX_
```

## 头文件布局

```c++
前置声明
类——声明
类——定义-功能实现
```

# class1——complex

##  类的声明

![image-20210906160932608](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210906160932608.png)

## inline——内联函数

增加了 inline 关键字的函数称为“内联函数”。内联函数和普通函数的区别在于：当编译器处理调用内联函数的语句时，不会将该语句编译成函数调用的指令，而是直接将整个函数体的代码插人调用语句处，就像整个函数体在调用处被重写了一遍一样。([链接](http://c.biancheng.net/view/199.html))

inline是C++关键字，在函数声明或定义中，函数返回类型前加上关键字inline，即可以把函数指定为内联函数。这样可以解决一些频繁调用的函数大量消耗栈空间（栈内存）的问题。关键字inline必须与函数定义放在一起才能使函数成为内联函数，仅仅将inline放在函数声明前面不起任何作用。inline是一种“用于实现”的关键字，而不是一种“用于声明”的关键字。([链接](https://baike.baidu.com/item/inline/10566989))

```c++
inline 函数返回值类型 函数名()
{
    
}
```

## class访问级别(access level)

```c++
public——放所有函数(几乎)
private——数据
protected——受保护的(暂时不说)
```

数据一定要通过类中的函数来传递出去(或者被设定)。除非这些数据是public,但我们要避免的就是这种。

![image-20210906162631950](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210906162631950.png)

## 构造函数(ctors)

1. 与类名相同
2. 可以有默认参数
3. 没有返回类型

## 构造函数特有语法

（充分运用特殊写法）

**注意**：括号中要有接收参数double r ,double i 

```c++
class complex
{
public:
    //一个变量的赋值使用有两个阶段，
    //1.初始化
    //2.赋值，使用
    //如果不用构造函数的特殊写法，就相当于跳过了初始化，直接在函数中赋值了
      complex(double r = 0,double i = 0):re(r),im(i)
          //传进来的两个数赋到类内变量中
    {
    }
private:
    double re,im;
};
```

**例**：

```c++
#include<iostream>
using namespace std;
class Test
{
public:
	Test(int c,int d):a(c),b(d)
	{

	}
	int FanHui()
	{
		return a;
	}
private:
	int a, b;
};
int main(void)
{
	Test s1(1, 2);
	cout << s1.FanHui() << endl;
	return 0;
}
```

与构造函数对应的析构函数，不带指针的类，大多不用写析构函数。

构造函数可以有很多个——overloading(重载)，同名的函数可以同时存在(在编译器看来其实不同名)，函数重载长长发生在构造函数上，但是这种不行

```c
class complex
{
public:
    complex(double r = 0,double i = 0):re(r),im(i)
    {
    }
    complex():re(0),im(0)
    {
	}
private:
    double re,im;
};
```

## 构造函数放到private?

什么情况会这样做？

不允许外接创建对象，那这个类有什么用呢？‘

**例**:设计模式中的**Singleton**(单体)

```c
class A
{
public:
	static A& gentInstance();
    setup(){......}
private:
    A();
    A(const A& rhs);
    ...
};
A& A::getInstance()
{
    static A a;
    return a;
}
```

## 常量成员函数const member functions

class里面的函数分为会改变数据内容的，和不会改变数据内容的，不会改变数据的内容的函数**加const**。

加不加const看传进来的参数经过这一系列运算会不会发生改变。(全局函数同理)

```c++
double real()const{    return re;}
```

**该加const的位置一定要加const**——创建类对象前面加了const

![image-20210906170251208](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210906170251208.png)

## 参数传递

**首先考虑传引用，注意看是否可以传引用。**

**pass by value** 

```c++
正常参数传递 类型 名称
```

**pass by reference (to const)**

```c++
就是传引用,相当于传指针。效果也同样相同    const complex&   ————传引用加const防止被修改
```

## 返回值

**首先考虑传引用，注意看是否可以传引用。**

**return by value**

```c++
类型
```

**return by reference (to const)**

```c++
引用
```

**什么时候不能返回引用**

如果新得到的结果放到了已经有的空间位置上，就OK。(如图)

但是，将两个已有的数据加在一起，不能放到原来已经有的位置上，这时候就需要在函数中创建一个新的变量用来接收的这个新得到的值，这时候不能返回这个新创建的变量，因为局部变量( local变量)在函数结束之后就消失了。

![image-20210906174311493](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210906174311493.png)



## 友元friend

![image-20210906173119933](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210906173119933.png)

## 同一个class的各个object互为友元

![image-20210906173635470](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210906173635470.png)

## 操作符重载operator overloading(成员函数)

**操作符重载通过成员函数或者非成员函数实现**。

c++中操作符就是一种函数，因为它可以重新定义。

所有的成员函数都带着一个隐藏dispointer,指向调用者。

**传递者无需知道接收者是以reference形式接收**

![image-20210906194143586](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210906194143586.png)

## 操作符重载operator overloading(非成员函数)

为了应对client_使用者的不同用法，我们这里给出3种写法。

**注意到**：运算的数写的位置不同，所重载的版本不同。

并且这几个绝对不可return by reference,因为他们返回的必定是local object，不是赋值给了已经存在的空间位置上，而是从这个函数中创建出一个complex，然后将它返回。 

**是从哪里创建的呢？**

**这里有个特殊语法**：

**typename()**;——**创建临时对象**，它的生命到下一行就结束了。

![image-20210906195013096](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210906195013096.png)

**重载返回值的特殊情况**：

注意到连用情况，在本次重载<<运算符中，如果client_user按照标准库中的cout使用方式连用，那么我们重载所设置的返回值就还得是个ostream类型，因为它从左向右运算，完成第一个之后，得到的类型还得是ostream类型才能接受这个<<。

**但是**,如果client_user不连用，只是cout<<xxx;那么本次运算之后的返回值是什么就无所谓了，我们可以填个void,并且注意，没有return。

![image-20210906202138200](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210906202138200.png)

# complex类实现过程

**注意**：成员函数实现重载，作用在运算符左边。传进来的参数是另一个。

**简单实现**：

**MyComplex.h**

```c++
#ifndef mycomplex
#define mycomplex
#include<iostream>
using namespace std;

class MyComplex
{
public:
	MyComplex(double r = 0, double i = 0) :re(r), im(i) {}
	double real()const//得到实部
	{
		return re;
	}
	double imag()const//得到虚部
	{
		return im;
	}
	MyComplex& operator += (const MyComplex& c);//一个复数加到另一个复数上

	bool operator == (const MyComplex& c);//判断两个复数是否相等
private:
	double re, im;
	//在函数前加friend变为友元函数，写在这里，这个函数就能取到这个类的私有数据了。
	
	friend MyComplex& GetBody1(MyComplex* ths, const MyComplex& tempC);
};

//将一个虚数的实部和虚部加到另一个上
inline MyComplex& GetBody1(MyComplex* ths, const MyComplex& tempC)
{
	ths->re += tempC.re;
	ths->im += tempC.im;
	return *ths;
}
//两个虚部进行相加
/*为什么要把对两个复数的运算操作写成成员函数？
因为可以利用到this指针，书写简单*/
inline MyComplex& MyComplex::operator+=(const MyComplex& c1)
{
	return GetBody1(this, c1);//注意到this是个指向调用者的指针，所以上面的GetBody1的参数1类型要写MyComplex*
}



//重载<<运算符，在屏幕上打印输出负数
inline ostream& operator << (ostream & os,const MyComplex& c1)
{
	return os << '(' << c1.real() << ',' << c1.imag() << ')';
}

//两个复数类的相加运算，并且加到一个新的复数类上
inline MyComplex operator + (const  MyComplex& c1,const MyComplex& c2)
{
	return MyComplex(c1.real() + c2.real(), c1.imag() + c2.imag());
}

//一个复数和一个double数进行运算
inline MyComplex operator + (MyComplex& c1, double x)
{
	return  MyComplex(c1.real() + x, c1.imag());
}

//对这个复数进行取反
inline MyComplex operator - (MyComplex& c1)
{
	return MyComplex(-c1.real(), -c1.imag());
}

//判断两个复数是否相等
inline bool MyComplex::operator == (const MyComplex& c)
{
	return this->im == c.im && this->re == c.re;
}

//得到一个复数的共轭复数
inline MyComplex GetRatherImag(const MyComplex& c)
{
	return MyComplex(c.real(),-c.imag());
}
	

#endif


```

**MyComplex.cpp**

```c++
#include "mycomplex.h"
#include<iostream>
using namespace std;

int main(void)
{
	MyComplex c1(1, 2);
	MyComplex c2(-1, -2);
	MyComplex cTemp1();


	
	cout << c1 << endl;
	cout << c1 + c2 << endl;
	cout << c1 << c2 << endl;
	cout << c1 + 1 << endl;
	cout << -c1 << endl;
	c1 += c2;
	cout << c1 << endl;
	cout << (c1 == c2) << endl;
	
	cout << GetRatherImag(c2) << endl;
	c1 = c2;
	cout << c1 << endl;

	return 0;
}


```

# class2——string

class带指针，我们就不能用默认的拷贝构造函数，应该自己写。

因为拷贝了指针，这两个指针指向的是同一块内容空间。

![image-20210907204221502](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210907204221502.png)



## class with pointer  members

只要了类带着指针，就要写拷贝构造和拷贝赋值，析构函数。——Big Three三个特殊函数。

**class里面有指针，多半是要动态内存分配。**

因为传递的是指针，创建的这两个class中的data就是一个指针，如果就使用编译器的否拷贝构造函数。

那么就会使得这两个指针指向的是同一块内存空间。

并且会导致另一块没有指针指向的内存空间引起**memory leak**(内存泄漏)。

——这就是我们所说的**浅拷贝**。

![image-20210907210003827](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210907210003827.png)



## 深&浅拷贝ctor function

 引出——**深拷贝**，我们coder所要写的这个(copy ctor)拷贝构造函数。

因为它的名字和类名相同，所以他是构造函数，并且它的参数传递的是它本身这种类型，所以叫做copy ctor。

拷贝构造应该做什么?
为传进来的这个蓝本创建足够的空间。

```c++
inline 
String::String(const String& str)
{
    m_data = new char[strlen(str.m_data)+1];
    strcpy(m_data,str.m_data);
}
```

```c++
String s1("Hello");
//以下两行代码意思完全相同
String s2(s1);
String s2 = s1;
```

## copy assignment operator——拷贝赋值函数

将原来的内容清空，开辟一块与另一块内存一样大的空间，然后将另一块的内容复制到这块来。

self assignment——检测自我赋值

![image-20210908182935320](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210908182935320.png)

# string类的实现过程

(不管什么函数就都加inline就完事了，编译器会做决策)

**MyString.h**

```c++
#ifndef myString
#define myString//注意防卫式声明的名字不能和类名相同！

#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
#include<cstring>
using namespace std;

class MyString
{
public:
	MyString(const char* cstr = 0);//接收什么样的初值
	//拷贝构造
	MyString(const MyString& str);

	//拷贝赋值
	MyString& operator = (const MyString& str); //看函数返回出来的结果要放到什么地方去，来决定是否可以Return by reference

	//(指针指向c字符的字符串是C语言风格的字符串)

	//返回字符串丢到cout
	char* get_str()const
	{
		return m_data;
	}

	//析构函数
	~MyString();

private:
	char* m_data;//放一根指针，动态分配
};

inline MyString::MyString(const char* cstr)
{
	if (cstr != 0)
	{
		m_data = new char[strlen(cstr) + 1];
		strcpy(m_data, cstr);//将传进来的内容拷贝到新分配到的空间中
	}
	else
	{
		//没有初值也要来一个空间
		//放/0,就是——所谓空字符串也要有一个最后的结束符
		m_data = new char[1];
		*m_data = '\0';
	}
}

inline MyString::~MyString()
{
	delete[] m_data;//arry new 配 arry delete
}

inline MyString::MyString(const MyString& str)
{
	m_data = new char[strlen(str.m_data) + 1];
	strcpy(m_data, str.m_data);
}

inline MyString& MyString::operator = (const MyString& str)
{
	//注意参数中的& 和if中str前的& ，他们两个的意义是不同的。

	//考虑到是否进行自我赋值
	//来源端和目的端是否相同

	if (this == &str)//取地址得到的是一根指针
	{
		return *this;
	}


	delete m_data;
	//清理掉之后重新分配一个够大的空间
	m_data = new char[strlen(str.m_data) + 1];
	strcpy(m_data, str.m_data);

	//考虑到连串情况(类似于之前MyComplex中cout<<连用的情况)
	//所以返回MySring& 
	return *this;
}

#endif // !MyString


```

**MyString.cpp**

```c++
#include"MyString.h"
#include<iostream>
using namespace std;
int main(void)
{
	//MyString s1("hello");
	//cout << s1.get_str();
	string stemp = {};
	MyString s2("");
	cout << s2.get_str() << endl;;
	cout << sizeof(s2.get_str()) << endl;
	cout << sizeof(stemp) << endl;
	//cout << "1" << endl;

	MyString s3("在");
	s2 = s3;
	cout << s2.get_str() << endl;


	return 0;
}
```



# 堆、栈、内存管理

## Stack

是存在于某个作用域的一块内存空间memory space。

调用函数，就会形成一个stack,用来存放它的参数，以及返回地址。

**Stack object**的生命周期在作用域结束之后就结束了。 会被自动清理。

## Heap

即——System Heap，是指由操作系统提供的一个global内存空间，由coder负责分配。

```c++
complex c1(1,2);complex *p =  new complex(3);
```

**Static  object **在作用域结束之后仍然存在，直到整个程序结束 ，析构函数才被调用。

## New

**new完记得Delete。**

调用New，先分配一块内存空间，然后再调用构造函数。

**例如**：

```c++
调用
MyComplex *pc = new MyComplex(1,2);
之后
我们可以理解为
实际上编译器转化为了3条语句
    
    分配内存-相当于调用malloc(n)
void* temp = operator new(sozeof(MyComplex));
pc = static_cast<MyComplex*>(temp);//转型
pc->MyComplex::MyComplex(1,2);//调用构造函数
```

## Delete

**先调用析构函数，再释放内存。**与New相反

```c++
delete pc;

Complex::~Complex(pc);
operator delete(pc);
				（即 调用free）
```

##  arry new 

arry new一定要搭配arry delete

即

```c++
String *str = new String[3];
...
delete[] p;//调用三次dtor
```

否则会造成内存泄漏。

**注意泄漏的是哪个位置**。

![image-20210908193839238](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210908193839238.png)



# static_静态

C与C++的static有两种用法：[面向过程](https://baike.baidu.com/item/面向过程)程序设计中的static和[面向对象程序设计](https://baike.baidu.com/item/面向对象程序设计)中的static。前者应用于普通变量和函数，不涉及类；后者主要说明static在类中的作用。——[链接](https://baike.baidu.com/item/static/9598919)



在有的情况，例如银行的利率，我们就可以将它设置为静态static类型，因为每个人看到的利率都是一样的，只有一份。



静态函数没有dispointer，所以它只能处理静态的数据。



**静态的数据一定要在class外定义**。

  ```c++
class Account
{
public:
	static double m_rate;
    static void set_rate(const double &x)
    {
        m_rate = x;
	}
};
double Account::m_rate = 8.0;
  ```

调用静态函数有两种方式

1. 通过object对象调用
2. 通过class name调用

**有时我们的class只希望创建一个对象**

只有有人调用到这个函数，这个对象才会诞生。

没人用，这个唯一的对象就不会产生。only one forever

```c++
class A
{
public:
    static A& getInstance();
    setup(){};             
private:
    A();
    A(const A& rhs);
    ...
};
A& A::getInstance()
{
    static A a;
    return a;
}

```

# cout补充

![image-20210908213824015](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210908213824015.png)

# class template_类模板

![image-20210908214533298](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210908214533298.png)

# function template_函数模板

![image-20210908214827122](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210908214827122.png)

 函数模板不必明确的指出来传入的类型，编译器会进行推导。

与运算符重载相互搭配。

(C++标准库里的算法都是function template形式)

# namespace

```c++
namespace std
{
    
}
```

使用方式

1. using directive_打开全部封锁
2. using declaration_一条一条声明
3. 啥也不写，每条都得std::xx

# class Composition_复合

a has b 

(标注库中的很多容器就是复合)

例如：将deque容器改装成 queue——adapter_改造

![image-20210909085401344](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210909085401344.png)

**构造由内而外——**包饺子

**析构由内而外**——剥桔子

# class Delegation_委托

Composition by reference

**编译防火墙**

![image-20210909091044631](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210909091044631.png)

# Inheritance_继承

is a

**构造由内而外——**包饺子

**析构由内而外**——剥桔子

继承——子类会有父类的part

**主要搭配virtual用**

# Inheritance with virtual functions 

在成员函数前+virtual。

数据和函数都可以被继承下来，只不过函数继承的是调用权。

**成员函数分为三种**：

1. non-virtual 函数:你不希望子类(dericed class) 重新定义(override覆写——之能用在虚函数被重新定义)
2. virtual函数:你希望被dericed class override,并且它已有默认定义。
3. pure virtual函数:你希望dericed class**一定**要override,且它没有默认定义

![image-20210909094754866](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210909094754866.png)

子类对象可以调用父类函数

![image-20210909122137728](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210909122137728.png)

(例如MFC框架)

**只有应用程序本身知道如何读取自己的文件(格式)**

# Inheritance + Composition下的构造和析构

**当一个类继承了另一个类，并且又复合了一个类，那么他们的构造函数和析构函数的调用顺序是什么样的呢？**

test.h

**这里面的Person3继承了Person1,复合了Person2**

```c++
#ifndef Test
#define Test
#include<iostream>
using namespace std;

class Person1
{
public:
	Person1(int r = 0):m_age(r)
	{
		cout << "Person 1的默认构造函数" << endl;
	}
	~Person1()
	{
		cout << "Person 1的析构函数" << endl;
	}
private:
	int m_age;

};

class Person2
{
public:
	Person2(int r = 0):m_age(r)
	{
		cout << "Person 2的默认构造函数" << endl;
	}
	~Person2()
	{
		cout << "Person 2的析构函数" << endl;
	}
private:
	int m_age;
};

class Person3:Person1
{
public:
	Person3(int r = 0):m_age(r)
	{
		cout << "Person 3的默认构造函数" << endl;
	}
	~Person3()
	{
		cout << "Person 3的析构函数" << endl;
	}
private:
	int m_age;
	Person2 p2;
};



#endif // !Test


```

**test.cpp**

```c++
#include"test.h"
#include<iostream>
using namespace std;
int main(void)
{
	Person3 testTemp;
	
	return 0;
}
```

**结果**：

![image-20210909124703540](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210909124703540.png)

**如果一个类继承了一个父类，并且这个父类中复合了一个类呢，那么他的构造函数和析构函数的调用顺序是如何的呢？**

这里的Person1继承了Person3,Person3复合了Person2

**test.h**

```c++
 #ifndef Test
#define Test
#include<iostream>
using namespace std;



class Person2
{
public:
	Person2(int r = 0):m_age(r)
	{
		cout << "Person 2的默认构造函数" << endl;
	}
	~Person2()
	{
		cout << "Person 2的析构函数" << endl;
	}
private:
	int m_age;
};

class Person3
{
public:
	Person3(int r = 0):m_age(r)
	{
		cout << "Person 3的默认构造函数" << endl;
	}
	~Person3()
	{
		cout << "Person 3的析构函数" << endl;
	}
private:
	int m_age;
	Person2 p2;
};

class Person1 :public Person3
{
public:
	Person1(int r = 0) :m_age(r)
	{
		cout << "Person 1的默认构造函数" << endl;
	}
	~Person1()
	{
		cout << "Person 1的析构函数" << endl;
	}
private:
	int m_age;

};

#endif // !Test


```

**test.cpp**

```c++
#include"test.h"
#include<iostream>
using namespace std;
int main(void)
{
	Person1 tempTEST;
	return 0;
}
```

**结果如图**：

![image-20210909125648963](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210909125648963.png)

# Delegation  +  Inheritance 委托+继承

![image-20210909130434009](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210909130434009.png)

(面向对象问题——准备class来解决问题)

**Composite**

![image-20210909132300696](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210909132300696.png)

**Prototype**

![image-20210909152011127](/images/侯捷C-面向对象高级编程(上)笔记.assets/image-20210909152011127.png)

