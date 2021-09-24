---
title: C++虚函数知识点总结
date: 2021-09-24 18:00:08
tags:
  - -C++
  - -笔记
categorise:
  - C++
cover: /img/c++.png
---

# 虚函数

**注意**：

- 在函数声明的返回类型之前加virtual。

- 并且只在函数的声明中添加virtual,在该成员函数的实现中不用加。

****

## 虚函数的继承

- 如果某个成员函数被声明成虚函数，那么他的子类，以及子类中的子类 ，所计继承的这个成员函数，也自动是虚函数。
- 如果在子类中重写这个虚函数，可以不用再加virtual,但仍然建议加上virtual，提高代码的可读性。

***

## 虚函数原理——虚函数表

对应虚函数的类，该类的对象所占内存大小为，数据成员的大小+一个指向虚函数表指针 (4字节)。

**例如**：如下所示Father类所创建的对象

```c++
class Father
{
public:
	virtual void func1()
	{
		cout << "虚函数func1" << endl;
	}
	virtual void func2()
	{
		cout << "虚函数func2" << endl;
	}
	virtual void func3()
	{
		cout << "虚函数func3" << endl;
	}
	void func4()
	{
		cout << "非func4" << endl;
	}
public:
	int x = 200;
	int y = 300;
	static int z;
};
int Father::z = 0;
```

```c++
Father father;
cout<<sizeof(father)<<endl;
```

**结果为12**，两个int的数据成员4+4一共占了8个字节，再加上一个虚函数表指针(4个字节)，一共是12个字节

( 如果该类中没有虚函数，就没有虚函数表指针，也就少4个字节)

**如下图所示**:

![image-20210923162312616](/images/C++虚函数知识点总结.assets/image-20210923162312616.png)

![image-20210923175606957](/imges/C++虚函数知识点总结.assets/image-20210923175606957.png)

**思考**：它尽然是个指针，那我们就能通过这个指针来访问它所指向内存所对应的内容。

(先存的是虚函数表指针，然后才是数据成员。)

**所以说**，对象地址就是虚函数表地址。

```c++
cout<<(int*)&father<<endl;
```

强转成指针。

**接着**，取出虚函数表的指针。

```c++
int* vptr = (int*)*(int*)(&father);
```

为了编译器能通过，前面加上int*。

**然后**，就找到了虚函数，并执行方法。

为了便于调用，这里定义个函数指针类型。

```c++
typedef void(*func_t)(void);
```

func_t指针，指向参数为void，返回值为void的函数。

调用虚函数。

```c++
((func_t)*(vptr))();
((func_t)*(vptr + 1))();
((func_t)*(vptr + 2))();
```

**调用成功**。

接着调用x,y两个数据成员。

```c++
cout << *(int*)((int)&father+ 4) << endl;
cout << *(int*)((int)&father+ 8) << endl;
```

取到地址，转成int整数，加上偏移量，通过编译器加上(int*),再解引用，得到里面的值。

(+上偏移量要先转成int)

**多态的使用**：**父类指针指向子类对象**

```c++
Father* father1 = &son;
father1->Func1();//调用对应的func1函数，son中的
```



##  使用继承的虚函数表

在上面的基础上，为Father类添加一个派生类。并且对Father的func1进行重写，再添加一个它独有的func5,声明为虚函数。

```c++
class Son :public Father
{
public:
	virtual void  Func1()
	{
		cout << "Son Func1()" << endl;
	}
	virtual void Func5()
	{
		cout << "Son Func5" << endl;
	}
};
```

![image-20210923180943284](/imges/C++虚函数知识点总结.assets/image-20210923180943284.png)

**同上面通过使用指向虚函数表的指针来访问对应的内容**

```c++
for (int i = 0; i < 4; i++)
	{
		//取到这个地址的内容，然后通过自定义指针类型转换，调用该函数,加()
		((func_t) * (vptr + i))();
	}
```

```c++
// 访问两个成员
cout << *(int*)((int)&son + 4) << endl;
cout << *(int*)((int)&son + 8) << endl;

```

## 子类虚函数表

1. 直接复制父类的虚函数表

   ![image-20210924120520593](/imges/C++虚函数知识点总结.assets/image-20210924120520593.png)

2. 如果子类重写了父类的某个虚函数，那么就在这个虚函数表中进行相应的替换

   ![image-20210924121032927](/imges/C++虚函数知识点总结.assets/image-20210924121032927.png)

3. 如果子类中添加的新的虚函数，就把这个虚函数添加到虚函数表中(尾部添加)

<img src="/imges/C++虚函数知识点总结.assets/image-20210924121136461.png" alt="image-20210924121136461" style="zoom:;" />

## 使用多重继承的虚函数表

**在上面的基础上再添加一个Mother类**

```c++
class Mother
{
public:
	virtual void handle1()
	{
		cout << "Monther handle1" << endl;
	}
	virtual void handle2()
	{
		cout << "Monther handle2" << endl;
	}
	virtual void handle3()
	{
		cout << "Monther handle3" << endl;
	}
public://便于测试，所以权限定为public
	int m = 400;
	int n = 500;

```

**此时的Son类对象**

vs编译器中把子类自己的虚函数放到了第一个父类的虚函数表最后

![image-20210924123011972](/imges/C++虚函数知识点总结.assets/image-20210924123011972.png)

**同样通过指针访问对应的虚函数表内容**

```c++
Son son;
cout << (int*)&son << endl;
//第一个虚函数表指针
int* vptr1 = (int*)*(int*)&son;
for (int i = 0; i < 4; i++)
{
	((func_t)*(vptr1 + i))();
}
// x y 
for (int i = 0; i < 2; i++)
{
	cout << *(int*)((int)&son + 4 + 4 * i) << endl;
}

//第二盒个虚函数表指针
int* vptr2 = (int*)*((int*)&son + 3);//取出来的是指向第二个虚函数表的指针 
for (int i = 0; i < 3; i++)
{
	((func_t)*(vptr2 + i))();
}
//m n
for (int i = 0; i < 2; i++)
{
	cout << *(int*)((int)&son + 16 + i * 4) << endl;
}
```

***

**小补充**：

**对象地址+偏移量**

转化int类型 + 对应的字节个数

转化int*类型 + 走几步(几个步长)

***

## 虚函数的修饰

### final

**final**——C++11更新

1.用来修饰类，让该类不能被继承。

```c++
class XiaoMi
{

};
class XiaoMi2 final:XiaoMi
{

};
class XiaoMi3 :XiaoMi3//报错——XiaoMI2不能被继承
{

};

```

(**补充**:C++默认继承方式为private)

2.用来修饰虚函数，使得该虚函数在子类中，不得被重写。但是还可以使用。

### override

override仅能修饰虚函数。

只能用在函数的声明，函数的实现不要写。

**作用**：

1. 提示程序的阅读者，这个函数是重写父类的功能。
2. 防止程序员在重写父类的函数时，把函数名写错。

##  父类的虚析构函数

把father类的指针定义为virtual时，并且对父类的指针执行delete操作时, 就是对该指针使用"动态析构"。

如果这个指针指向的是子类对象，那么会先调用该子类的析构函数，再调用父类的析构函数。

如果指向的是父类对象，那么只调用父类的析构函数。 

**注意**：
为了防止内存泄露，最好在基类的虚构函数上添加virtual关键字，使基类析构函数为虚函数。

## 纯虚函数与抽象类

**什么时候使用纯虚函数**？

某些类，现实项目和实现角度吗，都**不需要实例化**(不需要创建它的对象)。

这个类中定义的某些成员函数只是为了提供一个形式上的接口，准备让自子类来做具体的实现。

此时这个函数就可以定义为"**纯虚函数**"，包含纯虚函数的类，就叫做**抽象类**(不能创建对象)。

继承该抽象类的子类如果不重写这个纯虚函数，那么它也是不能创建对象的。

**用法**：
virtual  +  = 0

**代码示例**:

```c++
#include<iostream>
#include<string>
using namespace std;
class Shape
{
public:
	Shape(const string& color = "White")
	{
		this->color = color;
	}
	virtual float area() = 0;
	~Shape()
	{

	}
private:
	string color;
};

class Circle :public Shape
{
public:
	Circle(float radius = 0, const string& color = "White") :Shape(color), r(radius)
	{

	}
	virtual float area()
	{
		return 3.14 * r * r;
	}
	~Circle()
	{

	}
private:
	float r;
};
int main(void)
{
	Circle c1(3);
	cout << c1.area() << endl;
	return 0;
}
```

**纯虚函数的注意事项**：
父类声明为某纯虚函数之后,它的子类：

1. 实现这个纯虚函数
2. 继续把这个纯虚函数声明为纯虚函数，这个子类也称为抽象类
3. 不对这个纯虚函数做任何处理，等效于上一种情况(不推荐)

