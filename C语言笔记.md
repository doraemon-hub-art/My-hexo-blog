---
title: C语言笔记
date: 2021-03-09 08:05:43
tags:
	- -C语言
	- -笔记
categories:
	- C语言
cover: /img/C语言.png
---

# 1.类型

​	（1）整型int

​	（2）字符型char

​	（3）实型（浮点型）

​				单精度实型float

​					双精度实型double

精确到第几位？

例如%。3f，就是小数点后保留三位

​		（4）unsigned int ——无符号整型

# 2.输入与输出  scanf printf

每一个scanf函数调用都紧跟在一个printf函数调用的后面，这样做可以提示用户何时输入，以及输入什么。

##         （1）输入函数scanf

​							例：num1把那个值放在那个地址处

清空输入缓冲区

如果在scanf("%d,%d,%d",&a,&b,&c)

这双引号里面有逗号，那么在控制台之内输入的时候就要以逗号隔开

双引号中间的叫做格式化字符串，&叫做取地址符。



## 			（2）输出函数printf

printf("",)格式化字符串  输出列表

### 1.使用Printf函数输出八进制和十六进制数

```c
转换符号“%o”、“%X”
```



### 2.域宽

指的是存放输出数据的宽度

# 3.Main函数

(1)主函数程序的如空有且只有一个

（2）int main (mian前面的int表示main函数调用返回一个整型值)

（3）void main 已经过时

（4）main放在哪里都行，都先执行他

# 4.打印的类型

## （1）%d 

（十进制有符号整数）

例如%-6d——输出的整型占6个字符宽

-号为左对齐符号，默认为向右对齐



%u 十进制无符号整数
%f 浮点数
%s 字符串

%c 单个字符
%p 指针的值
%e 指数形式的浮点数
%x, %X 无符号以十六进制表示的整数
%0 无符号以八进制表示的整数
%g 自动选择合适的表示法

# 5.计算所占字节数量

计算元素个数

sizeof(arr)/sizeof(arr[0])

sizeof



# 6.计算机中的单位

(1) bit 比特位

(2) byte字节

(3) kb

(4) mb

(5) gb

(6) tb

(7) pb

# 7.变量

（1）定义变量的类型：类型 变量名 赋值

（2）{}大括号

（3）定义在{}之外的叫做全局变量

  (4)全局变量和局部变量可以同时存在，并且局部变量优先

（5）局部变量和全局变量的名字最好不要一样，容易产生bug

  (6)局部变量只能在他所在的那个局部使用

（7）c语言语法规定，变量要定义在当前代码块的前面

# 8.常量

## （1）字面常量

3： 100： 3.14：

## （2）const修饰的常变量

​     	（const常属性）

```C
const int num = 4; 
```



```c
const int n = 10; 

//n是变量 但是又有常属性,所以我们称n是常变量
```

##     (3)#define定义的标识符常量

## （4）枚举常量

```C
//枚举---一一列举
//性别：男 女 保密
//三原色：红黄蓝
```

枚举关键字---enum

```c
enum Sex
{
    MALE,
    FEMALE,
    SECRET
        //这三个叫做枚举常量
}；
int main()
{
    printf("%d\n",MALE);
    printf("%d\n",FEMALE);
    printf("%d\n",SECRET);
	return 0;
}

```

# 9.字符串

例子：名字 身份证号...

==不能用来比较两个字符串是否相等，应该用一个库函数-strcmp

（1）就是双引号引出的东西

```c
int main()
{
    "abcdef";
    "hellow sb"
    "";//空字符串
}
```



```c
int main()
{
    char arr1[] = "abc";
    //数组
    printf("%s\n",arr1);
    return 0;
}
```

(2)字符串的结束标志是一个\0的转义字符，在计算字符串长度的时候\0是结束标志，不算作字符串的内容 。

```c
int main()
{
    //数据在计算机存储的时候，存储的是2进制
    //#av$
    //a - 97 
    //A -65
    //  
    //ascii 编码
    //ascii 码值
    char arr[] = "ABC" ； //数组
     //"ABC" -- 'A' 'B' 'C' '\0' -- '\0'
       //字符串的结束标志
     char arr2[] = {'A' ,'B', 'C', '\0'};
    //'\0' - 0
    //'A' - 97
    // 'A' 'B' 'C'
    printf("%s\n",arr1);
    printf("%s\n",arr2);
    
    return 0;
    
}
```

# 10.计算字符串长度

strlen()

计算字符串长度时\0是结尾的标志，不算做字符数量

**只有从前往后数的时候碰到\0才会停止**

# 11.转义字符

（转变原来的意思）



​	    (1).\n 换行

​		(2).\t 水平制表符

​					(类似于Tab)

​		（3）.\? 

​					（在连续书写多个问号的时候，防止他们被解析成三字母词）

​						(在c语言早期的时候有个东西叫做三字母词---？？+)

​		（4）.\+任意带有转义字符的字母，使其不被解析为转义字符

​		（5）\转义任意，例：'/'  转义两个单引号中后面的那个，使其打印出来的就是一个

```c
#include <stdio.h>
#include <string.h>
int main（）
{
    printf("%d\n",strlen("c:\test\32\test.c));
    return 0;
}
                         //此输出结果为13
```

（6）\ddd   ddd表示一道三个八进制数字

例子：上框中 32作为8进制代表的那个十进制数字，作为ascii码值，对应的字符

​       32=====》10进制 26======》作为ASCII码值代表的数

（7）\xdd   dd表示2个16进制的数字

# 12.库函数

```c
//(1)printf
 #include <stdio.h>
//(2)字符串操作函数
  #include <string.h>
//(3)引入数学公式
 #include <math.h>
//(4)引入system
#include<stdlib.h>

//(5)

//(6)

//
```

# 13.注释

```c
//(从c++引进的)
/*  */  (C原本的注释，开始 只要在碰到*/就会停止)
```

# 14. if   while语句

# 15.函数举例

例

```c
int a = 100；
int b = 200;
sum = Add(a, b) 
    //f(x) = 2*x+1
    //f(x,y) = x+y
```

 

```c
int Add (int x, int y)
{
    int z = x+y;
    return z;
}
int main()
{
 int num1 = 10;
 int num2 = 20 ;
 int sum = 0;
 int a = 100;
 int b = 200;
 sum = Add(num1,num2)
 sum = Add(a,b) 
 printf("SUM = %d\n" , sum)
     return 0;
}
```

# 16.自定义函数

# 17.数组

**一组相同元素的集合**

数组的大小要拿常量来指定

```c
int main()
{
	//int a = 1;
   // int b = 2;
    //int c = 3;
    //int 4 = 4;
    
    
    int arr[10] = {1,2,3,4,5,6,7,8,9,10};
    //定义一个存放10个整数数字的数组
    //char ch[20];
   // float arr2[5];
    return 0;
  
```

(1)下标	

语法规定 数组的下标从0开始

![image-20201121115222649](https://i.loli.net/2020/11/29/2CmWdisDt1Hxe6p.png)

```c
printf("%d\n",arr[下标]);
//直接访问对应数组的元素

```

```c
//数组中
int arr[] = {1,2,3,4,,5,6};
//这里的int指的是数组中各个元素的类型，并不是数组的类型，
//数组的类型较为复杂
//求数组中的元素个数
printf("%d\n",sizeof(arr) / sizeof(arr[0])); 
```



# 18.操作符

## （1）算数操作符

## （2）移位操作符

移的位是二进制位

左移<<                 右移>> 

```c
int main ()
{
    int a = 1;
    //整型1站四个字节 ——32个bit位
    //000000000000000000000000000000001
    //四个字节放的就是这个二进制序列 
    //左边丢弃右边补零
    
    
    
}
```

![image-20201121154224594](https://i.loli.net/2020/11/29/H9MjELVF7pdbe13.png)

## （3）位操作符

这个位还是二进制位

&按位与

|按位或

^按位异或

```c
//按位与
//--对应的二进制要与（并且）一下 
//c语言中0为假，非0就是真
int main()
{
    int a = 3; //011
    int b = 5;	//101
    int c = a&b; //001  
    //只有两个都为1按位与出来才是1
    printf("%d\n",c);
}
输出结果为 //1(1)  (换为十进制)
```

```c
//按位或
int main()
{
    int a = 3;  //011
    int b = 5;	//101
    int c = a|b; //111
    //只要有一个不是0按位或出来就是1
    printf("%d\n",c);
}
输出结果为//7
```



```c
//按位异或
//计算规律 对应的二进制位相同则为0
//         对应的二进制位相异则为1 
int main()
{
    int a = 3;  //011
    int b = 5;	//101
    int c = a|b; //110
    
    printf("%d\n",c);
}
输出结果为//6
```



## （4）赋值操作符

​      [1]

```c
int main()
{		
    int a =10;
    a = 20;//赋值 // ==判断相等
 	//a = a+10;  (给a加10在赋给a)等价于 
    //a += 10;
    例子
      //a = a - 20;
        a -= 20 ；
      //a = a & 2;
        a &= 2;
    return 0;  
    //符合赋值符
    //+= -= *= ......
}
```

## （5）单目操作符

  	双目操作符

​	三目操作符

```c
int main()
{
  	int a  = 10;
    int b = 10;
    a + b;//+双目操作符
    return 0;
    
    ！---逻辑反操作
    int a = 10;
    //在c语言中我们表示真假
    //0 表示假 一切非0 表示真 
    printf("%d\n"!a )
        //为真 固定输出为一个数字1
    
}
```

​    sizeof 计算的是变量/类型所占空间的大小

单位是字节

![image-20201121162503188](https://i.loli.net/2020/11/29/yxUpEYLumiVh4HN.png)

sizeof  计算int不可省略括号 计算a可以省略括号



~对一个数的二进制进行按位取反

```c
#include <stdio.h>
int main()
{	
    int a = 0;//四个字节 32bit
    int b = ~a;
    //按位（二进制位）取反
    //1010  变成
    //0101	 
    printf("%d\n",b);
 	return 0;    
}
//原码 反码 补码
//负数在内存中存储的时候，存储的是二进制的补码
//使用的打印的是这个数的原码
//先补码-1得到反码 ，反码按位取反变成原码
```





-- ++

```c
//-- ++ （--++的值都是1 ）
#include <stdio.h>
int main()
{
    int a = 10;
    int b = a++;//后置++,先使用,再++
    // int b = ++a;前置++,先++，再使用
    //后置--  先使用 再--
    printf("a = %d\n b = %d\n", a ,b);
    return 0 ;
}
```



```C
// *
```

```c
//(类型)---强制类型转换 
#include <stido.h>
int main()
{	
    int a = (int) 3.14;//我要把3.14强制转换 3.14是double类型的
    				//这里让他强制转化为int型的
    return 0;
    
}
```





## （6）关系操作符

比较大小 看关系是什么  

大于等于>=

不等于！=

## （7）逻辑操作符  

```c
//&&逻辑与 必须两个都是非0的 一0一非0输出也为0
#include <stido.h>
int main()
{
    //真 -非0
    //假 - 0
    // 
    int a = 3;
    int b = 5;
    int c = a&&b;
 	printf("c = &d\n",c);
    return 0;
}
//
```



```c
//||逻辑或 a和b里有一个为真就行  
//他俩都为假的时候才为假 都是真的那都是真的
#include <stdio.h>
int main()
{	
    int a = 10;
    int b = 0;
    int c =  a||b;
   
    
    return 0;
}
```

## （8）条件操作符（三目操作符）

比较抽象

exp1? exp2 : exp3;

```c
//    exp1? exp2 : exp3;
#inculde <stdio.h>
int main()
{	
    int a = 100;
    int b = 20;
    int  max = 0;//存储
   
    max = (a > b ? a : b )
        //如果a>b这个表达式的结果为真
   //则max 为a  不管如何max都放的是ab的等较大值
    if (a > b)
        max = a ;
    else 
        max = b ;
    
    
    
    return 0;
}
```

## （9）逗号表达式

epx1,exp2,exp3....expn

用逗号隔开表达式

## （10）下标引用操作符

```c
#include <stdio.h>
//数组中
int main()
{	
    int arr[10] = { 0 };
    arr[4] ;//[]下标引用操作符
    return 0;
}
```



## （11）函数调用操作符

```c
#include <stdio.h>
int Add( int x, int y)
{	
    int z =0;
    z = x + y;
    return z;
    
}
int main()
{	
    int a = 10;
    int b = 20;
   	int sum =  ADD(a,b); //()-函数调用操作符
    
    return  0;

}
```





# （12）解引用操作符

（13）

（14）

# 19.复制基础框架

```C
#include <stdio.h>
int main()
{	
   
}
```





# 20.综合练习

## （1）求两个数的较大值

```c

```

## (2)定义一个函数来求两个数的较大值



```c
#include <stdio.h>
int Max (int x, int y) //接收a b
{     //大括号为函数体
    if ( x > y )
        return x;
    else 
        return y;
}
int main()
{ 
    int a = 10;
    int b = 20;
    int max = 0;
    max = Max(a,b);
    //定义一个名为Max的函数
    printf("max = %d\n",max);
    
    return 0 ;
    
}
```

#  21.     原码、  补码、反码

```c
只要是整数 内存中存储的都是二机制的补码
    对于//正数来说，他的原码补码反码相同
   		//负数 存的是补码
    	原码是直接按照正方写出的二进制数列
    	反码从原码的符号位不变其他位取反得到
    	补码 反码+1
    
```



#    ![image-20201122213736103](https://i.loli.net/2020/11/29/sPKAugpDYBOeoQc.png)

# 22.常见关键字

关键字不能与变量名冲突

关键字不能自己创建

关键字不能做变量名

## （1）static

static可以修饰局部变量

可以修饰全局变量

可以修饰函数

-2![image-20201122214135006](https://i.loli.net/2020/11/29/h6HuSskTFBWyRZI.png)

![image-20201130164931937](C:/Users/xuanxuan/Desktop/C笔记.assets/image-20201130164931937.png)

```c
#include <stdio.h>
int main()
{	
    int a = 10;//局部变量-自动变量（省略auto）
    
    return 0;
}
```

```c
//register 
#include <stdio.h>
int main()
{	
   // register int a =10;
    //建议把a定义成寄存器变量
    int a = 10;
    a = -2;
    int 定义的变量是有符号的
        (signed) int ;
    unsigned int num = 0;
    无符号为没有正负
    return 0;
}
```

```C
//
#include <stdio.h>
int main()
{	
   //struce -结构体关键字
}
```

```c
#include <stdio.h>
 int main
           //typedef-类型定义-类型重定义
     typedef unsigned int u_int
```

```C
//static-用来修饰局部变量
//修饰局部变量时局部变量的生命周期变长
#include <stdio.h>
void test()
{
    static int a = 1; //a为一个静态的局部变量
    a++;
    printf("a = %d\n",a);
}
//出这个函数a不销毁了
int main()
{	
    int i =0;
    while (i<5)
    {
        test();
        i++;  
    }
    return 0;
}
```

![image-20201123092942576](https://i.loli.net/2020/11/29/qgJ91dckfthulMo.png)

```c
//static修饰全局变量
//改变了变量的作用域 
//让静态的全局变量只能在自己所在的原文件内部使用
//出了原文件就无法使用了
#include <stdio.h>
int main()
{	
    //extern -声明外部符号的
    extern int g_val ;
    printf("g_val = %d\n", g_val);
    return  0;
}
```

```c
//static修饰函数也是改变了函数的作用域
//这种说法不准确
//正确说法是
//static修饰函数 改变了函数的链接属性
//外部链接属性==>内部链接属性
#include <stdio.h>
//在外部 static  int Add(int x, int  y)
//        z = x + y;
//	     return z;
声明外部函数
    extern int Add(int,int);
int main()
{	
    int a = 10;
    int b = 20;
    int sum = 	Add(a,b);
    printf("sum = %d\n")
    return 0;
}
```

# 23.#define定义常量和宏

预处理指令，不是关键字

```c
#include <stdio.h>
//#define定义的标识符常量
//#define MAX 100

//#define可以定义宏-带参数

//函数的实现
int Max(int x,int y)
{
	if(x > y)
  		return x;
    else
         return y;
}
//宏的定义方式
#define MAX(X,Y) X>Y?X:Y
int main()
{	
    //int a = MAX；
   	int a = 10;
    int b = 20;
    //函数
    int max = Max(a,b);
    pritnf("max =%d\n",max);
    //宏的方式
    max = MAX(a,b);
    //max = (a>b?a:b);
    printf("max = %d\n",max);
    return 0;
```

# 24.指针

指针——是个变量 用来存放地址

每个小格子的编号——地址

（1）如何产生地址

​		32位 ——有三十二跟地址线/数据线

​		正负电10之分

​	电信号转化为为数字信号



%p用来打印地址 

​           

```c
#include <stdio.h>
int main()
{	
    int a = 10;//给a申请4个字节
    //只要是个值就可以存起来
     int* p = &a;
    //p现在是一个指针变量 
    //p的类型是int*
    //p里面存的是a的地址
    //有一种变量是用来存放地址的——指针变量
    //&a;//取地址
  //  printf("%p\n",&a);
 //  printf("%p\n,p");
    //*p,这颗*就是解引用操作符/间接访问操作符
    *p = 20;
    printf("a = dp\n",a);
    return 0;
}
```

​            

```c
int a = 10;创建变量a a的地址中放了10
int *p = &a; 创建p变量存a的地址
*p = 20;对p里面存的地址进行操作，通过p找到a
    	*p就是a,把a的10改成了20
```

​               

例子

//地址就是二进制序列

```c
#include <stdio.h>
int main()
{	
    char ch = 'w';
    char*pc = &ch;
    *pc = 'a';
    printf("%c\n",ch);
        //打印字符%c
	return 0;
}
 
```

​                                                

```c
//指针大小32位平台4个字节，64 位平台8个字节
#include <stdio.h>
int main()
{	
    char ch = "w";
    char*pc = &ch;
    printf("%d\n",sizeof(pc));
    
    return 0;
}
```



```c
#include <stdio.h>
int main()
{	
    
    return 0;
    
}
```



```

```





# 25.结构体

如何描述复杂对向——结构体

自己创造出来的一种类型

struct——结构体关键字

定义===》初始化

```c
#include <stdio.h>
//改名字的库函数
#include <string.h>

//创建一个结构体类型
struct book
{
    //数的类型
    char name[20];//c语言程序设计
    short price;
    
};
//分号结束类型定义
int main()
{	
   //利用结构体类型创建一个该类型的结构体变量出来
    struct book b1 = {"c语言程序设计"，55};
    struct book*pb = &b1;
    
    
    //利用pb打印一下书名和价格
    printf("%s\n", pb->name);
    printf("%d\n", pb->price);
    //.操作符应用于结构体变.成员
   //->结构体指针 ->成员
    
    
   /*intf("%s\n",(*pb).name);
	printf("%s\n",(*pb).price);*/
    
    
   // printf("书名：%s\n",b1.name);
    //printf("价格：%d\n",b1.price);
    b1.price = 15;//改变价格 price为变量
    		//name不可这样改
    //strcpy字符串拷贝 库函数
    //改名字 stycpy关于字符串操作
    stycpy(b1.name,"C++")
   	printf("修改后的价格：%d\n",b1.price);
    
    return 0;
}
```

# 26.分支语句和循环语句

结构 

顺序结构 选择结构 循环结构

什么是语句：用分号隔开的就是，有一个分号算一个，直写一个分号也算一条语句——空语句，

一对大括号就是一个代码块

注意if和else的匹配

else和离他最近的if相匹配



## （1）分支语句（选择结构）

### if语句

1.后面加大括号{}可以输入多条语句，否则只能输入一跳语句

2.if语句中0表示假，非0表示真

3.if语句是一种分支语句可以实现单分支，也可以实现多分支

 4.else与他最近的（未匹配的if）相配对

```c
//单分支if
#include <stdio.h>
int main()
{	
    int age = 10;
    if(age<18);
    	printf("未成年\n");
    return 0;
}
```

```C
//if else
#include <stdio.h>
int main()
{	
    int age = 10;
    if(age<18);
    	printf("未成年\n");
    else
        print("成年\n");
    return 0;
}
```

```c
//
#include <stdio.h>
int main()
{	
    int age = 10;
    if(age<18);
    	printf("未成年\n");
    else if(age>=18 &&age<28)
        printf("青年\n");
    else if(age>=28 &&age<50)
        printf("壮年\n");
    else if(age>=50 &&age<90)
        printf("老年\n");
    else  
        printf("老不死")；
    return 0;
}
```



```C
#include <stdio.h>
int main()
{	
    int age = 10;
    if(age<18);
    	printf("未成年\n");
    else
    {
    	if(age>=18 &&age<28)
        printf("青年\n");
    else if(age>=28 &&age<50)
        printf("壮年\n");
    else if(age>=50 &&age<90)
        printf("老年\n");
    else  
        printf("老不死")；
    }
    return 0;
}
//同上
```

```c
// =赋值 ==判断相等
int main()
{	
    int num = 1;
    if(5 == num) //这样写 出现错误可以立即显示 便于发现与改正
    {
        printf("sb\n");
    }
    return 0;
}
```

### 练习

```c
判断一个数是否为奇数 输入1-100之间的奇数
#include<stdio.h>
#include<string.h>
 //我的错误示范
/* int main
{	
    int a = 0;
    printf("请输入一个数字");
    scanf("%d\n",&);
    if(a%2 == 0)
        printf("不是");
   	else
        printf("是");
    return 0;
}
```



```c
//正确
//方法
#include <stdio.h>
int main()
{	
    int i = 1;
    while(i<=100)
    {
        if(i%2 == 1)
            printf("%d\n",i);
        i++;
    }
    return 0;
}
//方法2 
#include <stdio.h>
int main()
{	
    int i = 1;
    while(i<=100)
    {	
        if(i%2 !=0)
        	printf("%d", i);
        i++;
    }
    return 0;
}
```



### switch语句

分支语句 

switch（整型表达式）

搭配break实现分支

产生的结果必须为整型

case整型常量表达式 case后面必需为整型常量表达式，

不要求顺序

default字句可以放在任意位置





```c
#include <stdio.h>
//case入口 braek跳出语句
int main()
{
	int day = 0;
	scanf("%d", &day);
	switch (day)
	{
	case 1:
		printf("星期一\n");
         break;
	case 2:
		printf("星期二\n");
         break;
	case 3:
		printf("星期三\n");
         break;
	case 4:
		printf("星期四\n");
         break;
	case 5:
		printf("星期五\n");
         break;
	case 6:
		printf("星期六\n");
         break;
	case 7:
		printf("星期天\n");
         break;
	}
   
	return 0;
}
```

![image-20201123211047909](https://i.loli.net/2020/11/29/V9THD48ldSszfQb.png)

相同的case没必要每个后面都加上case,合到一起即可

switch语句中可以出现if

```c
//如果输入了1~7以外的数
//顺序没有严格的措施
default:
	 pritnf("输入错误\n")；
     break;

```

### 练习

```c
#include <stdio.h>
int main()
{
	int n = 1;
	int m = 2;
	switch (n)
	{
	case 1:
		m++;
	case 2:
		n++;
	case 3:
			switch (n)
		{
			case 1:
			n++;
			case 2:
			m++;
			n++;
			break;
		}
	case 4:
		m++;
	default:
		break;
	}
	printf("m = %d\n, n = %d\n", m,  n);
	return 0;
}
//5,3
```



## （2）循环语句

## while

条件表达式的执行次数总是比循环体的执行次数多一次

### 将大写字母转换为小写字母

```c
#include<stdio.h>
int main(void)
{	
    in ch = 0;
    
    while ((ch = getchar()) != EOF)
    {
       printf("%c\n",ch +32);
        getchar();//清理\n
  
    }
    return 0;
}
```



### break:

在循环语句中只要遇到break，就停止后期所有的循环，直接终止循环。所以:while中的break是用于永久终止循环的。

### continue：

是用于终止本次循环的，也就是本次循环中continue后边的代码不会再执行，而是直接跳转到while语句的判断部分。进行下一循环的入口判断。

```c
//条件为真就执行 循环执行 直至为假
#include <stdio.h>
int main()
{	
    while (1)
        printf("hehe\n");
    return 0;
}
```

```c
#include <stdio.h>
int main()
{	
    int i =1;
    while(i<=10)
    {	
        if(i == 5)
            //break;
            //1234
       //达到满足条件循环停止
            //continue;
            //1234 
        //回到while 陷入死循环
        printf("%d",i);
        i++;
    }
    return 0;
}
```



### getchar putchar

可以接受一个键盘的字符

```c
#include <stdio.h>
int main()
{	
    int ch = 0;
    //getchar遇到 CTRL+Z停止
    //EOF文件结束标志
    while((ch = getchar()) != EOF)
    {
        putchar(ch);
    }
    
    printf("%c\n",ch);
    return 0;
}
```



### 小练习

```c
//出错了
#include <stdio.h>
int main()
{	
    int ret =0;
    //接收
    char password [20] = {0};
    printf("请输入密码：>");
    scanf("%s",password);
    //输入密码 并存放在password数组中
    printf("请确认Y/N：>");
    ret = getchar();
    if(ret == 'Y')
    {
        printf("确认成功\n")；
    }
    else
    {
        printf("放弃确认\n")；
    }
    return 0;
}
```



## for（一小部分文件丢失——暂未补充）

语法for（exp1；exp2；exp3）

​				循环语句

exp1为初始化部分，用于初始化循环变量

exp2为条件判断部分，判断条件是否终止

exp3为循环调整，

把while循环中的三个部分放到了一起

```c
#include <stdio.h>
int main()
{	
    int i = 0;
    for(i = 1;i<=10;i++)
    {
        printf("%d",i);
    }
    return 0;
}
```



### for 语句的循环控制变量

(1)不可以在for循环体内修改循环变量，防止for循环失去控制，陷入死循环

![image-20201126201152688](https://i.loli.net/2020/11/29/fVl9D6ogEyKJscb.png)

（2）建议for语句的循环控制变量的取值采用“前闭后开区间”写法

for循环的初始化判断调整都是可以省略的

for(;;) 但是for循环的判断条件被省略的话，判断条件就恒为真，陷入死循环

如果不熟练，建议不要随便省略

```c
#include <stdio.h>
int main(void)
{	
    for(;;)
    {
        printf("HEHE")；
    }
    return 0;	
    
}
```



```c
#include <stdio.h>
int main(void)
{	
     int  i = 0;
     int j = 0;
    for(;i<10;i++)
    {
        for(;j<10;j++)
        {
            printf("hehe\n")
    return 0;
}
        
       //输出运行结果为十个hehe
```



执行流程

![image-20201126172057875](https://i.loli.net/2020/11/29/sKzcgl35FmDwC9v.png)

## do while

do语句

循环语句

```c
#include <stdio.h>
{
int i = 1;
do
{
    printf("%d",i);
    i++;
}
while(i<=10);
return 0;
}
```



do语句的特点：

循环至少执行一次，使用的场景有限，所以不是经常使用

## goto语句

c语言的goto语句在哪都可以使用；理论上goto语句手机没有必要的

最常见的用法就是终止程序中某些深层嵌套的结构处理过程，例如一次跳出两层或多层循环

```c
#inlcude <stdio.h>
int main(void)
{	
    again:
            printf("hello sb\n")；
                //goto放哪里就跳到哪里
            goto again;
            return 0;
}
/////////////////////////////////////
#include <stdio.h>
int main(void)
{	
    for ()
    {
        for()
        {
            for(
                {
                    if(disater)
                        goto errot;
                }
        }
    }
    error:
        if(disater)
                //处理错误情况 ，跳出循环，省去了使用break的麻烦
    return 0;
}
```



# 27.获取/显示字符

## 缓冲区:输入的东西会先被存放在缓冲区

输入缓冲区在键盘和getchar中间

## （1）getchar

在这里

在代码运行窗口输入的回车也会被当做字符而被获取

例如

```c
//这里输入一个1 结果：c1= 1 ,c2 = ,（\n）回车（换行符）
#include <stdio.h>
int main()
{
    char c1,c2;
    printf("请输入一个数字:\n");
    c1 =  getchar();
    c2 =  getchar();
    printf("c1 = %c.c2 = %c\n"，c1,c2);
    return 0;
}
```



## (2)putchar

```c
#include<stdio.h>
 int main()
 {	
     int ch = 0;
     while(ch = (getchar)) ! = EOF)
     {
         if (ch < '0' || ch > '9')
             continue;
         putchar(ch);
     }
     return 0;
 }
```



# 28.scanf与scanf_s

scanf_s是scanf的安全版本

# 29.综合练习（2）

## 1.计算n的阶乘

```c
//不考虑溢出的情况
#include<stdio.h>
int main(void)
{	
    int i = 0;
    int n = 0;
    int ret = 1;
    printf("请输入一个整数：");
   	scanf("%d",&n);
    for(i = 1;i <= n ;i++)
    {
        ret = ret * i;
    }
    printf("ret = %d",ret);
    return 0;
}
```

## 2.计算1~10的阶乘

在1题的基础上，在外面再套一个循环

```c
#include<stdio.h>
int main(void)
{	
    int i = 0;
    int n = 0;
    int ret = 1;
    int sum = 0;
    printf("请输入10：");
   	scanf("%d",&n);
   for(n =1 ;n<=10;n++)
   {
    for(i = 1;i <= n ;i++)
    	{
        ret = ret * i;
    	}
       sum = sum + ret;
   }
    printf("sum = %d",sum);
    return 0;
}
```

## 3.在一个有序数组在这中查找具体的某个数字n

```c
#include <stdio.h>
int main(void)
{	
    int arr[] = {1,2,3,4,5,6,7,8,9,10};
    int k = 7;
    //写一个代码，在arr数组中找到7
    int  i = 0;
    int sz = sizeof(arr)/sizeof(arr[0]);
    for(i = 0;i <sz;i++);
    {
        if(k == arr[i])
        {
            printf("找到了，下标是:%d\n",i);
            break;
        }
    }
    if (k == sz)
    {
        printf("找不到\n");
    }
    return 0;
}
```



若有n个元素，最坏的情况要找n次，

或者先找中间元素，逐渐缩小查找范围

## 4.编写代码掩饰多个字符从两端移动，向中间汇聚。

## 5.编写模拟登陆场景

并且只能登陆三次，只允许输入三次密码，如果密码正确则提示登录成功，如果三次均输入错误，则退出程序。

```c
#include <stdio.h>
int main(void)
{	
    
    return 0;
}
```

## 6.写三个数让他从大到小输出

```c
#include<stdio.h>
int main(void)
{	
    int a = 0;
    int b = 0;
    int c = 0;
    printf(" 请输入三个数：");
    scanf("%d%d%d",&a,&b,&c);
    //算法实现 
    //a中放最大值 b次之 C中放最小值
    if(a<b)
    {
        //临时变量 防止在将a的值赋给b时，a的值丢失
        int tmp = a; 
        a = b;
        b = tem;
    }
    if(a<c)
    {
        int tmp = a;
        a = c ;
        c = tmp;
    }
    printf("%d %d %d\n", a, b, c);
	return 0;
}
```



7.打印1~100所有3的倍数 

```c
#include<stdio.h>
int main(void)
{
    int i = 0;
    for(int i = 1;i <= 100;i++)
    {
        if(i%3 == 0)
        {
            printf("%d",i);
        }
    }
    return 0;
}
```

## 8.给定两个数求最大公约数

辗转相除法

```c
#include<stdio.h>
int main(void)
{	
	int n,r,m;
	printf("请输入两个大于零的整数：");
	scanf("%d %d", &m, &n);
	while (r = m%n)
	{
		m = n;
		n = r;
	}
	printf("%d\n", n);
	return 0;
}
```



## 9.打印1000~2000的闰年

1判断闰年的方法，能被4整除且不能被100整除

2能被400整除是闰年

```c
#include<stdio.h>
int main(void)
{
	int year;
	for (int year = 1000; year <= 2000; year++)
	{
		if (year % 4 == 0 && year % 100 != 0)
		{
			printf("%d\n", year);
		}
		else if (year % 400 == 0)
		{
			printf("%d\n", year);
		}
	}
	return 0;
}
```



![image-20201203160113405](C:/Users/xuanxuan/Desktop/2020.12.2.assets/image-20201203160113405.png)





## 10.打印100~200的所有素数

​		

```c
//试除法
#include<stdio.h>
int main(void)
{
    //定义整型并初始化
	int num = 0;
	int i = 0;
	int count = 0;

        //理解
        //让i从2开始
        //num开始被i除一直除到num-1
        //如果其中有num被i整除了，循环就终止，break
        //因为素数是除了1和他本身之外不能被其他数所整除
        //在这里只要i和Num不相等，num被其他说所整除，说明，num不是个属于素数，什么也不输出，1是默认的，可以将任意的num整除，在这里i从2开始，所以是素数的数只能被其本身所整除，即i = num,
    	for (int num = 100; num <= 200; num++)
	{
            //i <num
            //筛选——输出
            //有其他可以将num整除的数也将终止循环
		for (  i = 2; i < num; i++)
		{
			if (num%i == 0)
			{
				break;
			}
		}
            //跳出来
            //就是i把num-1的数字都试过了，都不能将num整除
            //那么就是剩下i = num的情况了，此时这个数肯定为素数
            // i  =num
		if (num == i)
		{
			printf("%d\n", num);
			count++;
		}
	}
	printf("一共有%d个质数\n", count);
	return 0;
}



///////////////////////////////////////////////////////////////////
///////////////////////////////////////////////////////////////////
///////////////////////////////////////////////////////////////////


//如果一个数可以写成a*b的形式，例如；c = a*b
//那么a和b中的一个数肯定小于等于开平方c
//sqrt——开平方的数学库函数—#include<math.h>
//方法2
```





## 11.求十个数的最大值

```c
#include <stdio.h>
int main(void)
{	
    int arr[] = {1,2,3,4,5,6,7,8,9,10};
    int max = 0;//最大值
    int i = 0;
    int sz = sizeof(arr)/sizeof(arr[0]);
    for(i = 0;- < sz;i++)
    {
        if(arr[i] > max)
        {
            max = arr[i]; 
        }
    }
    printf("max = %d\n"
ma*
           .0
        
      
        
    return 0;
}
```





## 12.求1+。。。。1/10的和









## 13.乘法口诀标的打印

```c
//在屏幕上输出乘法口诀表
//分析：9行，多上行行号就是多少，先确定行，再确定列
#include <stdio.h>
int main(void)
{
	int i = 0;//行
	//确定打印9行
	for (i = 1; i <= 9; i++)//循环-行数
	{
		//每行打印的东西
		int j = 1;
		for (j = 1; j <= i; j++)
		{
            //%2d-打印两位 如果没有两位就用空格补齐
            //%-2d  向左对齐
			printf(" %d * %d = %-2d"i, j, i*j);

		}
		printf("\n");
	}
	return 0;
}
```



## 14.二分查找

```c
#include<stdio.h>
int main(void)
{	
    
    return 0;
}
```



## 15.猜数字游戏

1程序运行 电脑随机生成一个数字 猜大猜小有提示 

2菜单 循环运行

3时间戳：当前计算机的时间减去计算机的起始时间，1970年0时0分0秒

```c
#include<stdio.h>
#include(stdlib.h)
int main(void)
{	
    
    return 0;
}
```



## 16.关机程序（goto语句）

```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
int main(void)
{	
    int input[20] = {0};
    //在一分钟内关机
    //shoutdown -s -t 60
    //system执行系统命令的
    system("shutdown -s -t 60");
again:
    printf("请注意。你的电脑在1分钟之内关机，如果输入我是sb，就取消关机\n请输入>:");
    scanf("%s",input);
    if(stramp(input,"我是sb") == 0 )//比较两个字符串-strcmp
    {
        system("shutdown -a")
    }
    else
    {
        goto again;
    }
	return 0;
}
```



## 17.水仙花数字

```c
//什么是水仙花数字——三位上的数字的立方和等于这个数字
//步骤；寻找100~999之间的数字，分解整数，并且验证每个数的平方，最后输出是水仙花数字的数
#include<stdio.h>
int main(void)
{
	int num, bai, shi, ge;//定义这个数字和他的百位，十位，个位，为整形
	printf("水仙花数有：\n");
	for (num = 100; num < 1000; num++)//定义数的区间——保证为三位数
	{
		bai = num / 100;//求出百位上的数
		shi = (num - bai * 100) / 10;//求出十位上面的数
		ge = num % 10;//求模——余数
		//验证个十百位上的立方和是否等于这个数
		if (num == bai*bai*bai +shi*shi*shi+ge*ge*ge)
			
			printf("%d\n", num);
		
		
	}
	return 0;
}
```



# 30.函数

## 1.函数是什么

函数是程序中的某一部分，负责执行某项特定任务，相对于其他代码，具备相对的独立性。

一般会有输入参数并有返回值，提供对过程的封装和细节的隐藏，这些代码通常被集成为软件库。

在C语言中函数分为库函数和自定义函数

## 2.库函数

C语言本身提供的函数叫做库函数

常用的c语言库函数有

- IO函数

- 字符串操作函数

- 内存操作函数

- 时间/日期函数

- 数学函数

- 其他库函数

  例子

  ### strcpy

```c
//strcpy
//源头要是比目的地字符串长
//必然会溢出
//必需保证源头数据比目的地数据短
//或者目的地的空间足够大
#include <stdio.h>
#include <string.h>
int main(void)
{
	//\0是字符串的结束标志
	int arr1[] = "sb";
	int arr2[20] = "%%%%%%";
	//目标地址 来源地址 、
	//目的地的内容会被覆盖
	strcpy(arr2, arr1);
	printf("%s\n"arr2);
}
```

### memset

```c
#include<stdio.h>
#include<string.h>
int main(void)
{
	char arr[] = "hello world";
	//将arr中的前五个字符替换成*
	memset(arr,' *' , 5);
	printf("%s\n",arr);
	return 0;
}
//***** world
```





## 3.自定义函数

函数的基本组成：函数名 函数参数 返回类型 函数体

### (1)定义一个函数求两个数的最大值

```c
#include <stdio.h>
//定义函数
get_max(int x , int y)
{
    if(x > y )
        return x;
    else
        return y;
}
int main(void)
{	
    int a = 10;
    int b = 20;
    //函数的使用
    //a传给x,b传给y
    //传过去什么就用什么接收
    int max = get_max(a,b);
    printf("max = %d\n",max);
    return 0;
}

```



### (2)写一个函数可以交换连个整型变量的内容

```c
//错误示范——在主函数中实现两个整型变量的交换
#include <stdio.h>
int main(void)
{	
    int a = 10;
    int b = 20;
    int c = 0;
    printf("a = %d b = %d\n",a,b);
    c = a;
    a = b;
    b = c;
    printf("a = %d b = %d\n",a,b);
    return 0;
}
```



```c
//定义函数但是失败了的示范
#include <stdio.h>
//自定义函数
//前面加上void意思是此函数没有返回值
//什么类型就用什么类型接收
void exchange(int x, int y)
{
	//定义一个临时变量用于交换	
	int c = 0;
	c = x;
	x = y;
	y = c;
	//此时执行失败，在这个定义函数的区块中，交换是成功了，但是x和a，y和b
	//两组之间没有联系
}
int main(void)
{
	int a = 10;
	int b = 20;
	printf("a = %d b = %d\n", a, b);
	//函数的使用
	exchange = (a, b);
	printf("a = %d b = %d\n", a, b);
	return 0;
}
```



```c
//正确示范
#include <stdio.h>
void exchange(int x,int y)
{
    
}
int main(void)
{	
    int a = 10;
    int b = 20;
    printf("a = %d b = %d\n",a,b);
    return 0;
}
```



```c
//在这里引入指针的概念
#include <stdio.h>
int main(void)
{	
	int a = 10;
	int *pa = &a;//pa的类型就是int* ,指针变量
		//pa里面存的是a的地址
		//加个*意思是解引用操作
	*pa;//对pa进行解引用操作的意思是，通过pa里面存的这个地址，
		//找到它所指向的内容，那这个*pa就是a，
		//如果给 20
	*pa = 20;
		//就借助这个指针变量把a的值改成了20
	printf("%d\n", a);
		//也就是说，如果要改变a的值，不用a亲自动手，指针变量也可以修改
		//
	return 0;
}
```



```c
//上面出错的函数示范加上上面引入的指针概念，即可完成此代码的修改
//因为上面出错的原因是因为a与x，b与y，两对字母之间没有直接的关联，导致数据无法传递过去
思路：将a和b的地址传过去，因为通过地址就可以找到代码
    将地址传过去，就要用指针来接收
	#include <stdio.h>
    //传过来地址，要用指针来接收
     void exchange(int* pa,int* pb)
{
    int c = 0;
    c = *pa;
    //将a备份到c中
    //然后改变a
    *pa = *pb;
    *pb = c;
}
    int main(void)
{	
    int a = 10;
    int b = 20;
    printf("a = %d b = %d\n",a,b);
    exchange(&a,&b);
    printf("a = %d b = %d\n",a,b);
    return 0;
}

```



## 4.函数参数

### 实际参数（实参）

真实传递给函数的参数

实参可以是：常量、变量、表达式、函数等。无论实参是何种类型的量，在进行函数调用时，他们都必须拥有确定的值（能够算出确切的值），以便把这些值传送给形参。

### 形式参数（形参）

一般情况下不存在

形式参数是指函数名后括号中的变量，因为形式参数只有在函数被调用的工程中才实例化（分配内存单元），所以叫形式参数。形式参数当函数调用完成之后就自动销毁了。因此形式参数只在函数中有效。就是就收函数的x和y只在此函数中有效。



当实参传给形参的时候，形参其实是实参的一份临时拷贝，形参和实参之间没有联系，这时候，对形参的修改是不会改变形参的，例如上面失败的交换两个数值。

结论：形参实例化之后其实相当于实参的一份临时拷贝





## 5.函数调用

### 传值调用

函数的 形参和实参分别占有不同的内存块，对形参的修改不会影响实参

### 传址调用

传址调用是把函数外部创建变量的内地址传递给函数参数的一种调用方式。

这种传参方式可以让函数和函数外边的变量建立起真正的联系，也就是函数内部可以直接操作函数外部的变量。

### 什么时候用什么

如果只需要传递个值不用作出改变——传值

要改变函数外部的某些变量——传址

### 练习

#### （1）写一个函数输出100~200之间所有的质数

```c
//输出100到200之间的质数
#include <stdio.h>
#include <math.h>
//是素数返回1，不是素数返回0
int is_prime(int n)
{	
	int j = 0;
    //可以优化一下J<=sqrt(n)
	for (j = 2; j < n; j++)
	{
		if (n%j == 0)
			return 0；
            //不能太着急，应让其检验完毕
	}
    return 1;
}
int main(void)
{
	int i = 0;
	for (i = 100; i <= 200; i++)
	{
		//0判断I是否为质数
		if (is_prime(i) == 1)
			printf("%d\n", i);
	}
	
	return 0;	
}
```



#### (2)写一个函数打印1000~2000之间的闰年

```c
//写一个函数输出1000~2000之间的闰年
#include <stdio.h>
//返回的值是什么就在函数定义前加入什么
int is_runyear(int y)
{
	if ((y % 4 == 0 && y % 100 != 0) || (y % 400 == 0))
		return 1;
	else
		return 0;
}
int main(void)
{
	int year = 0;
	for (year = 1000; year < 2000; year++)
	{
		//不要忘了下面调用函数中（）中的内容，判断什么要写好，要不代码无法运行
		if (is_runyear(year) == 1)
			printf("%d\n", year);
	}
	return 0;
}
//函数在设计的时候，功能要单一，要干净
```



#### (3)二分查找（折半查找）

```c
////二分查找
#include <stdio.h>

	//二分查找
   //在一个有序数组中查找具体的某个数
	//如果找到了返回，这个数的下标，找不到返回-1

	//例如我要在这个数组中找到7
	//首先找到这组被查找元素的中间的元素
	//假如说发现中间元素5要比我要找的数要小
	//说明我要找的数在5的右边，这样我的范围就缩小了一半
	//查找了一次范围就缩小了一半，这样的速度是比较快的
	//这就叫二分查找(折半查找)
	//那么怎么找到中间元素的下标呢
	//原来的数组是1 2 3 4 5 6 7 8 9 10 
	//他们的下标是0 1 2 3 4 5 6 7 8 9
	//左下标为0，右下标为9
	//给定完这组元素的下标，就可以通过左右元素的下标来确定中间元素的下标
	//就是这组被查找元素的左下标是0，右下标是9
	//0和9的平均值是4，下标4锁定的元素是5，就可以认为5就是这组元素的中间元素了
	//5这个元素比我要找的7要小
	//说明我要被查找的元素范围就变成了6到10
	//新的范围，左下标是5，右下标是9
	//左右下标又可以求出一个平均值是7，又找到一个对应的元素是8
	//所以这一组查找范围的中间元素是8
	//用8再跟我要找的元素比一下，比我找的元素要大
	//说明我要查找的元素在8的左边
	//这时候要查找的范围被再次的缩小成了6到7


	//基本思想，当我确定了被查找范围时候，要确定他的左下标和右下标，然后求出下标的平均值，
	//找到中间元素，将中间元素和我要找的元素比一下，如果比我找的元素大，说明我要找的元素在他的左边，
	//如果比我要找的元素小，说明我要找的元素在他的右边，
	//这样确定出新的范围，出现新的左右下标。
	//一直找到左右下标无法确定新的范围，他们之间没有元素可以被查找的时候，结束，说明没有找到
	//如果在某一次查找的时候，找到了，下标相等了，说明找到了，把下标给过来

int number_search(int arr[], int k)
{
	//算法实现
	int sz = sizeof(arr) / sizeof(arr[0]);
	int left = 0;//左下标
	int right = sz - 1 ;//右下标
	//放到循环中
	while (left <= right)//这样才说明中间是有元素可以被查找的
	{
		int mid = (right + left) / 2;//中间元素的下标
		//拿到这个mid——锁定个元素
		if (arr[mid] < k)//如果中间元素比我要找的k要小，
						//被查找范围的右下标不用变，左下标变成mid+！
		{
			left = mid + 1;
		}
		else if (arr[mid] > k)
		{
			right = mid - 1;
		}
		else
		{
			return mid;
		}
		//如果找不到
		return -1;//返回去了
	}
}


int main(void)
{
	
	int arr[] = { 1,2,3,4,5,6,7,8,9,10 };
	int k = 7;
	int ret = number_search(arr, k);
	if (ret == -1)
	{
		printf("找不到指定的数字\n");
	}
	else
	{
		printf("找到了，下标是：%d\n",ret);
	}
	return 0;
}

//运行发现找不到结果——代码出现了问题
//自己找问题的方法
```



```c
//部分位置添加注释后
 ////二分查找
#include <stdio.h>

int number_search(int arr[], int k)
//实际上这地方传递过来的数组arr是首元素地址
//本质上这里的arr是个指针，因为指针才可以接收地址
{
	//算法实现
	int sz = sizeof(arr) / sizeof(arr[0]);
	// 4/4=1  无法得到元素个数
	//sz出现了问题
	int left = 0;//左下标
	int right = sz - 1;//右下标
	//放到循环中
	while (left <= right)//这样才说明中间是有元素可以被查找的
	{
		int mid = (right + left) / 2;//中间元素的下标
		//拿到这个mid——锁定个元素
		if (arr[mid] < k)//如果中间元素比我要找的k要小，
						//被查找范围的右下标不用变，左下标变成mid+！
		{
			left = mid + 1;
		}
		else if (arr[mid] > k)
		{
			right = mid - 1;
		}
		else
		{
			return mid;
		}
		//如果找不到
		return -1;//返回去了
	}
}


int main(void)
{

	int arr[] = {1,2,3,4,5,6,7,8,9,10};
	int k = 7;
	//只要把数组传参传过去
	//在内部是不能再上面的方式求元素个数了
	//数组是一块连续的空间，他里面可以放很多个元素
	//数组在传参的时候
	int ret = number_search(arr, k);//在这里仅仅传的是数组第一个元素的地址，不是所有元素
	if (ret == -1)
	{
		printf("找不到指定的数字\n");
	}
	else
	{
		printf("找到了，下标是：%d\n", ret);
	}
	return 0;
}
```

既然传进去不行，那就在外面算，

```c
//修改版(注释已经删除)(只有修改后的注释)
////二分查找
#include <stdio.h>
//将多传递过来的参数sz接收
int number_search(int arr[], int k,int sz)
{
	int left = 0;
	int right = sz - 1 ;	
    //进入到这个循环中就是一次二分查找
    //在这里要进行很多次
    //每一次二分查找的第一步是找被查找范围的中间元素的下标
	while (left <= right)
        //等于号不能丢
	{
		int mid = (right + left) / 2;
		if (arr[mid] < k)
		{
			left = mid + 1;
		}
		else if (arr[mid] > k)
		{
			right = mid - 1;
		}
		else
		{
			return mid;
		}
	}
    return -1;
}
int main(void)
{	
	int arr[] = { 1,2,3,4,5,6,7,8,9,10 };
	int k = 7;
    //将计算元素个个数
    int sz = sizeof(arr) / sizeof(arr[0]);
	int ret = number_search(arr, k,sz);//将sz也传过去
	if (ret == -1)
	{
		printf("找不到指定的数字\n");
	}
	else
	{
		printf("找到了，下标是：%d\n",ret);
	}
	return 0;
}

```

#### （4）写一个函数，实现，每调用一次这个函数，就会将num的值增加1

```c
//每次调用一次函数，num的值就会增加1

#include <stdio.h>
void Add(int *p)
{
	//这个时候的*p就是num
	//*p++;这样写是错误的
    //这里++的优先级要高一些
    //目的是让*p  ++
    //应该写成
    (*p)++;
}
int main(void)
{
	int num = 0;
	//从外面操作——传址
	Add(&num);
	printf("num = %d\n", num);//1
	Add(&num);
	printf("num = %d\n", num);//2
	Add(&num);
	printf("num = %d\n", num);//3
	return 0;
}
```









## 6.函数的嵌套调用和链式访问

### 嵌套调用

函数和函数之间是可以有机的组合在一起的

### 链式访问

把一个函数的返回值作为另外一个函数的参数

```c
#include<stdio.h>
int main(void)
{
	//求字符串的长度
		int len = 0;
	//第一种方法
		len = strlen("abc");
		printf("%d\n", len);
		//输出 3 
	//一句话搞定
		//这就是链式访问，像一个链条一样将函数有机的串在了一起
		printf("%d\n", strlen("abc"));
		//输出还是3
}
```

#### 例子：

下面这串代码会输出什么呢？

```c
//我以为就是输出43
//补充printf("",)这里面的内容分别是——格式化字符串——输出列表
//pirntf返回值是打印在屏幕上的字符数 
//但是答案却是4321
//
#include<stdio.h>
int main(void)
{	
	//printf的返回值是打印在屏幕上字符的个数
	printf("%d", printf("%d", printf("%d", 43)));
	//先打印 43 ——打印了两个字符——返回值是2——打印2-打印了1个字符——返回值1
	//printf("%d", printf("%d", printf("%d", 43)));
	//首先printf("%d", printf("%d", printf("%d", 43)))中最里面的printf开始打印
	//输出43——打印了两个是字符———那么他的返回值就是2
	//此时式子变成printf("%d", printf("%d", 2))
	//输出2——打印了一个字符——返回值是1
	//式子变成printf("%d", 1)
	//打印1——over
	return 0;
}
```



## 7.函数的声明和定义

### 函数的声明

1.告诉编译器有一个函数叫什么，参数是什么，返回类型是什么，但是具体是不是存在，无关紧要。

2.函数的声明一般出现在函数的使用之前。要满足先声明后使用。

3.函数的声明一般要放在头文件中的。

当函数定义放到主函数后面的话，代码无法运行报错

这里就用到了函数声明

```c
#include <stdio.h>
//函数声明
//在这个函数声明中的x和y是可以省略掉的，因为我们根本就不回去用他
int Add(int x, int y);
int main(void)
{	
	int a = 10;
	int b = 20;
	int sum = 0;
	//函调用
	sum = Add(a, b);
	printf("%d", sum);
	return 0;
}
//这时候把函数定义放到主函数的下面
//程序会报错——“Add未定义”
//虽然定义了但是程序并不知道
//这里就需要在主函数前面进行一个声明——函数声明
Add(int x, int y)
{
	int z = x + y;
	return z;
}
//这样写是比较啰嗦的

```



#### 函数声明正确的用法

创建三个文件add.h   add.c   test.c

自己写的头文件用双引号引

#include  " "   

在这里add.h和add.c共同构成一个加法模块

补充：在#include<stdio.h>中

当程序运行到这行是

程序会把相关有用的全部拷贝过来

```c
在add.h(头文件)文件中
    //写函数声明
#ifndef _ADD_H
    //如果没有定义过后面那个符号
    //那就先定义一下define
    //如果定义过了，就为假
    //下面的代码就不要了
    //防止用一个头文件被引用多次
#define _ADD_H
int Add(int ,int);
#endif
在add.C文件中
    //写函数的定义
 int Add(int x, int y)
{
    int z = x + y;
    return z；
}
在test.c文件中
    //直接进行用这个上面定义的加法函数
    #include <stdio.h>
    #include "add.h"
    int main(void)
	{
    	int a = 10;
    	int b = 20;
    	int sum = 0;
    	sum  =  Add(a,b);
        printf("%d",sum);
	}
  //为什么要这么弄呢？
//如果吧所有的代码函数都放到同一个test.c文件中
//项目组十个人分工，布置任务
//没法同时都在一个test.c文件中工作，
//这样分开工作，最后include引用,组合在一起
//功能复杂——分模块写
    
```

### 函数定义

函数的定义是指函数的具体实现，交代函数的功能实现。

## 8.函数递归

### 注意

递归必须要有结束条件，否则程序将崩溃

递归函数条件终止后就会逐层返回

。

### 什么是递归？

一个函数自己调用自己。把大事化小



详解：

程序调用自身的编程技巧成为递归。递归作为一种算法在程序设计语言中广泛应用。一个过程或函数在其定义或说明中有直接或间接调用自身的一种方法，它通常把一个大型复杂的问题层层转化为一个与原问题相似的规模较小的问题来求解，递归策略只需少量的程序，就可描述出解题过程所需要的多次重复计算，大大地减少了程序的代码量。递归的主要思考方式在于：把大事化小。

```c
//史上最简单的递归
#include<stdio.h>
int main(void)

{	
	//死循环的打印sbsb
	//走着走着挂了 
	//递归常见的错误——栈溢出
	//什么是栈溢出——
	//内存划分——栈区-堆区-静态区
	//	对应   放置-栈区-局部变量-函数形参
	//				堆区-动态开辟的内存  malloc calloc
 	//				静态区-全局变量-Staic修饰的变量
	printf("sbsb\n");
	main();
	return 0;
}
//stack overflow——栈溢出

	
```

#### 练习1

接收一个整型值（无符号），按照顺序打印它的每一位，例如：输入：1234，输出1 2 3 4 

```c
#include<stdio.h>
void print(int n)
{
	if (n > 9)
	{
		print(n / 10);
	}
	printf("%d ", n % 10);
}
//首先将输出的123传到n这里
//程序运行到if这里，进行判断,123>9条件成立，进行下一步,进入if
//接着将123/10 = 12余的3丢掉
//函数接着调用自己
//12>9条件成立，继续进行下一步，进入if
//再将12/10 = 1 余的2丢掉
//函数再次调用自己
//此时1>9条件不成立，进行到printf处，1%10 = 0余1，将1输出
//到现在，递归函数的条件已经终止(不成立)，开始进行逐层返回
//梳理一下层数
//1层 输入 123，
//2层 输入 12
//3层 输入 1
//4层 条件不成立 输出1
//由2层开始逐层向上返回
//2层 12%10 = 1余数 2，将2输出
//1层 123%10 = 12余数3，将3输出
//最后得出结果1 2 3 
int main(void)
{
	unsigned int num = 0;
	scanf_s("%d", &num);
	print(num);
	return 0;
}
```





### 递归的两个必要条件

1存在限制条件，当满足这个限制条件的时候，递归便不再继续。

2每次递归调用之后越来越接近这个限制条件。	

### 练习2 

编写代码，求字符串长度

strlen——求字符串长度

#### （1）不用函数的

```c
#include<stdio.h>
#include<string.h>
int main(void)
{	
	char arr[] = "bit";
	int len = strlen(arr);//求字符串长度
	printf("%d\n", len); 

	return 0;
}
```



（2）使用函数的（有临时变量——计数器）

```c
//这里面模拟实现了一个strlen函数
#include<stdio.h>
#include<string.h>
//传过来的是首元素的地址——char*-指针变量
int my_strlen(char* str)//求出的是长度——返回值是整型	
{
	//计算字符串的长度，count——计数器
	int count = 0;
	//不等于\0进入到循环之内
	//即没有到字符串的结束位置
	//当到\0处时，条件为假，调到return处
	while (*str !='\0')
	{
		count++;
		//str里面存的是地址，他+1就是 看下一位是什么
		str++;
	}
	return count;
	
		
}
int main(void)
{	
	//创建了一个数组，在这个数组里面放了，b i  t还有一个/0
	char arr[] = "bit";

	int len = my_strlen(arr);//arr数组传承那——传过去的不是整个数组，而是第一个元素的地址
	printf("len = %d\n", len);

	return 0;
}
```





无计数器的



```c
//无计数器
//无计数器的计算字符串的位数
//大事化小——看第一位是不是\0
//终止条件，
#include<stdio.h>
//bit\0
int my_strlen(char* str)
{
	if (*str != '\0')
		return 1 + my_strlen(str + 1); 
						//调整bit的所对应的位数 
	else
		return 0;
}
int main(void)
{	
	char arr[] = "bit";
	int len = my_strlen(arr);
	printf("%d", len);

	return 0;
}
```

### 递归 与迭代

迭代——类似于循环

#### 练习——求n的阶乘（不考虑溢出）

​		

```C
//计算n的阶乘——使用函数
//jiecheng这个函数没用递归
	#include<stdio.h>
	int jiecheng(int z)
	{
		int i = 0;
		int ji = 1;
		for (i = 1; i <= z; i++)
		{
			ji = ji * i;
		} 
		return ji;
	}
	//运用递归函数
	int jiecheng2(int z)
	{
		if (z <= 1)
			return 1;
		else
			return z * jiecheng2(z - 1);
	}
	int main(void)
	{	
		int n = 0;
		int ret = 0;
		printf("请输入一个数：");
		scanf_s("%d",&n);
		ret = jiecheng2(n);
		printf("上面这个数的阶乘是：%d",ret);
		return 0;
	}

```

求第n的斐波那契数（不考虑溢出）

```c
//求第n个斐波那契数
//斐波那契数列就是前两个数等于第三个数	
#include<stdio.h>
int fbn(int z)
{
	if (z <= 2)
		return 1;
	else
		return fbn(z - 1) + fbn(z - 2);
}
int main(void)
{	
	int n = 0;
	int ret = 0;
	scanf_s("%d", &n);
	//TDD——测试驱动开发，先想函数如何实现，然后再进行编写
	ret = fbn(n);
	printf("第这个的斐波那数是%d\n", ret);
	return 0;
}

//但是当输入50，想求第50个斐波那系数的时候，需要很长时间才能算出来，
//效率比较低，因为要求第50个斐波那系数，
//就要知道第48和第49个，要求他们两个，也同样要往前面推，求出两个，
//这都不是现成的，都需要机器先运算
//一层层的往下面拨，需要的数据太多了，2的次方数
//并且在这其中有很多的重复数据
//在此加入一个全局变量count来计算第三个斐波那系数被计算了多少次
```

```c
    加入全局变量count来计算3被计算了几次
    #include<stdio.h>
    int count = 0;
    int fbn(int z)
    {
        if (z == 3)//测试第三个斐波那系数被计算了多少次
        {
            count++;
        }
        if (z <= 2)
            return 1;
        else
            return fbn(z - 1) + fbn(z - 2);
    }
    int main(void)
    {
        int n = 0;
        int ret = 0;
        scanf_s("%d", &n);
        ret = fbn(n);
        printf("第这个的斐波那数是%d\n", ret);
        printf("第三个斐波那数被计算了——%d次\n", count);
        return 0;
    }
  

```

```c
上面递归的方法进行了太多的重复计算
这个用循环的方法
//解释
// 1 1 2 3 5 8 13 21 43 55 
//为了避免计算重复的是
//假设把第一个数的值赋给a,第二个数的值赋给b,
//将a+b的值赋给c
//循环往复
//其中前两个一加赋给第三个
//计算斐波那契数是从第三个开始
//创建循环的条件
#include<stdio.h>
int fbn(int z)
{
	int a = 1;
	int b = 1;
	//这是前两个斐波那契数
	int c = 0;
	//计算斐波那契数从第三个数开始
	while (z > 2)
	{
		c = a + b;
		a = b;
		b = c;
		z--;
	}
	return c;
}
int main(void)
{	
	int n = 0;
	int ret = 0;
	scanf_s("%d", &n);
	ret = fbn(n);
	printf("第%d个斐波那契数%d", n,ret);
	return 0;
}

```

#### 什么时候用递归什么时候用循环

#### 问题

当递归加上限制条件的时候，还是会栈溢出（stack overflow）

例如

```c
//跑一会儿程序就会奔溃
#incldue<stdio.h>
void test(int n)
{	
    if(n<10000)
    {
    	test(n+1)
    }   
}
int main(void)
{
    test(1)；
    return 0;
}
```

#### 递归的经典案例

1汉诺塔问题

2青蛙跳台阶问题

n个台阶，一次可以跳1个或者2个，请问有多少种跳法

《剑指offer》

# 31.数组

## 1.一维数组的创建和初始化

### 数组的创建：

数组是一组相同类型元素的集合，数组的创建方式

数组类型type_t 

数组名称arr_name

const_n是一个常量表达式，用来指定数组的大小

```c
#include<stdio.h>
int main(void)
{
	//创建一个数组用来存放10个整型
	int arr[10];
	char arr2[5];
	//char ch[n];——这样写不可以，数组的大小一定要用常量
	float arr3[10];
	double arr4[1];
	return 0;
}
```

### 数组的初始化

数组的初始化是指：在数组创建的同时给数组的内容一些合理初始值（初始化）。

```c
#include<stdio.h>
int main(void)
{
	int arr[10] = {1,2,3};//——这种叫不完全初始化，前三个是123，剩下的元素默认初始化为0
    //字符数组
	char arr2[5] ={'a','b（98）'};//初始化同样不完全，其中当把b替换成98时， 输出也是b，因为b的ascii码值就是98 
    char arr3[5] = "ab"；
    char arr4[] = "abcdef"；//不指定大小，进行初始化，他会根据初始化的内容，来确定数组几个元素
    printf("%d\n",sizeof(arr4));//7
    //sizeof——计算arr4所占空间的大小——放了abcdef\0这七个元素
    pritnf("%d\n",strlen(arr4));//6
    //strlen——求字符串长度，\0之前的字符个数，\0停止，\0不计数
    //////////////////////////////////////////////////////////
   补充知识
      strlen和sizeof没有什么关联
       strlen是求字符串长度的，想让他终止就得碰到\0,没有\o，计算出的值就是个随机值-只能针对字符串求长度——库函数-要引头文件
       sizeof计算变量、数组、类型的大小、-单位是字节、——操作符
  /////////////////////////////////////////////////////////////
       //完全初始化就是规定了数组中有几个元素，后面就初始化几个元素
       //数组在创建的时候如果不想指定
	float arr3[10];
	double arr4[1];
	return 0;
}
```



## 2.一维数组的使用

对于数组的使用我们之介绍了一个操作符：[]	,下标引用操作符，它其实就是数组访问的操作符，

```c
#include<stdio.h>
int main(void)
{	
	char arr[] = "abcdefe";//这里面放了[a] [b][c][d][e][f][\0]
	/*printf("%c\n", arr[3]);*///输出字符串中的第三个字符
	//这里的[]就是下标引用操作符
	int i = 0;
    int len = strlen(arr);
	for (i = 0; i < 6; i++)
        //下面这个同上
   // for(i = 0; i <(int)strlen(arr);i++)
       //for(i = 0; i< lenli++)
	{
		printf("%c ", arr[i]);//输出abcdef
	}
	return 0;
}

```



```c
#include<stdio.h>
int main(void)
{	
    char arr[] = {1,2,3,4,5,6,7,8,9,0};
    int sz = sizeof(arr)/sizeof(arr)[0];
    int i = 0;
    for(i = 0;i < sz ;i++)
    {
        printf("%c",sz);
    }
    return 0;
}
```

总结：

数组其实是用下标来访问的，下标是从0开始的。

数组的大小可以通过计算来得到。

## 3.一维数组在内存中的存储

一维数组在内存中的排布

```c
#include<stdio.h>
int main(void)
{
	int arr[] = { 1,2,3,4,5,6,7,8,9,10 };
	int sz = sizeof(arr) / sizeof(arr[0]);
	int i = 0;
	for (i = 0; i < sz; i++)
	{
		//输出此一维数组在内存中的排布情况
		printf("&arr[%d] = %p\n", i, &arr[i]);
		//在内存中连续开辟空间来存放1~10
		//数组在内存中是连续存放的
	}
	return 0;
}
```



数组在内存中是连续存放的

## 4.二维数组的创建和初始化

### 二维数组的创建

```c
数组的创建
    //3行4列的二维数组
int arr[3][4];
//3行5列
char arr[3][5];
//2行4列
double arr[2][4];
```

### 二维数组的初始化

```c
//这里创建一个3行4列的二维数组
//在第一行存放123没有初始化的位置放置0
//在第二行放置45，没有初始化的位置放置0
int arr[3][4] = {{1,2,3},{4,5}};
```

```c
二维数组要明确几行几列，列不能省略，行可以省略
```





## 5.二维数组的使用

## 6.二维数组在内存中的存储

5和6放到一起说了

```c
二维数组的使用也是通过下标的方式
 不管行还是列下标都是从0开始
     大致内容跟上跟一维数组一样
    //随之元素下标的增长，元素的地址也在有规律的增长，由此可以得出结论，数组再内存中是有连续存放的。
    二维数组在内存中也是连续存放的，像一维数组一样
    假想中是分行和列的，其实并不分，连续一条直线存放
    把二维数组拆成几个一维数组
    //二维数组
    arr[0][j]
    arr[1][j]
    arr[2][j]
    就可以想象成三个一维数组并排放置
#include<stdio.h>
int main(void)
{	
	int arr[3][4] = {{1,2,3},{4,5}};
	int i = 0;
	for (i = 0; i < 3; i++)
	{
		int j = 0;
		for (j = 0; j < 4; j++)
		{
			printf("%d ",arr[i][j]);  //打印此二维数组的所有元素
            printf("&arr[%d][%d] = %p\n", i, j, &arr[i][j]);//打印二维数组中各个元素的地址
		}
		printf("\n");
	}
	return 0;
}
```



## 7.数组作为函数参数

冒泡排序——将一个整型数组进行排序

```c
#include<stdio.h>
void maopao(int arr[] , int sz)
{	
	//确定冒泡的趟数
	//这里从下面传上来的数组只是数组中首元素的地址0
	int i = 0;
	
//int sz = sizeof(arr) / sizeof(arr[0]);//假设10个元素
	for (i = 0; i < sz - 1 ; i++)
	{
		//确定每趟中多少对比较
		int j = 0;
		//动态的
		for (j = 0; j < sz - 1 - i; j++)
		{
			if (arr[j] > arr[j + 1])
			{
				int tmp = arr[j];
				arr[j] = arr[j + 1];
				arr[j + 1] = tmp;
			}
		}
	}
}
int main(void)
{	
	int arr[] = { 10,9,8,7,6,5,4,3,2,1};
	int i = 0;
	int sz = sizeof(arr) / sizeof(arr[0]);//打印数组的每个元素
	//对arr数组进行冒泡排序
	//10个元素需要进行9趟冒泡排序
	//在外面算好元素个数然后传过去
	maopao(arr,sz);//调用冒泡排序
	for (i; i < sz; i++)
	{
		printf("%d ", arr[i]);
	}
	return 0;
}

```

```c
//若改为已经排好顺序的数组，代码依然会老老实实的进行冒泡排序
//优化
#include<stdio.h>
void maopao(int arr[] , int sz)
{	
	int i = 0;
	for (i = 0; i < sz - 1 ; i++)
	{
        int flag = 1;//假设这一趟要排序的数据已经有序
		int j = 0;
		for (j = 0; j < sz - 1 - i; j++)
		{
			if (arr[j] > arr[j + 1])
			{
				int tmp = arr[j];
				arr[j] = arr[j + 1];
				arr[j + 1] = tmp;
                flag  =  0 ; //本趟排序的数据其实并不完全有序
			}
		}
        if(flag == 1)
        {
            break;
        }
	}
}
int main(void)
{	
	int arr[] = { 10,9,8,7,6,5,4,3,2,1};
	int i = 0;
	int sz = sizeof(arr) / sizeof(arr[0]);
	maopao(arr,sz);
	for (i; i < sz; i++)
	{
		printf("%d ", arr[i]);
	}
	return 0;
}
//break只能用于循环或者if语句
```



数组名是什么？

```c
//打印的值是一样的但是意义是不一样的
printf("%p\n", arr);
printf("%p\n", &arr[0]); 
//前面两个代表的首元素地址
printf("%d\n", &arr);//整个数组的地址

```



```c
printf("%p\n", arr);
printf("%p\n", arr + 1);

printf("%p\n", &arr[0]);
printf("%p\n", &arr[0] + 1);

printf("%p\n", &arr);
pritnf("%p\n", &arr
```

补充：
1sizeof(数组名)，计算整个数组的大小，sizeof内部单独放一个数组名，数组名表示整个数组。

2……数组名，取出的是数组的地址。&数组名，数组名表示整个数组。

除了12的两种情况之外，所有的数组名都表示数组首元素的地址。

## 8.数组的应用实例1：三子棋

```c

```



## 9.数组的应用实例2：扫雷游戏	

# 32.操作符

操作符和表达式

## 1各种操作符的介绍

分类

### 算数操作符

```c
+ - * /
    
```

```c
//    / 
#include<stdio.h>
int main(void)
{		

	//注意其中取模操作符%的两边都要是整数
	//int a = 5 / 2; //商2余1  输出为2
	//int a = 5 % 2;  // 输出余数1
	double a = 5 / 2.0;//浮点型-让分子或分母任意一个为小数
	//输出就是2.500000
	printf("a = %f\n", a);
	return 0;
}
```

### 移位操作符

只能作用与整数

对于位移操作符，不要移动负数位数的，例如<<-1，这是标准未定义的

```c
//右移
#include<stdio.h>
int main(void)
{	
	//int a = 16;
	//16用二进制表示1000
	//16存在a中，a为整型，整型在内存中占4个字节，32个比特位
	//这个32个比特位位
	//这个空间就是单独创建出来给a的
	//00000000000000000000000000010000
	//拿着整个二进制数列往右边移动，
	//来检测一下这里是算数 右移还是逻辑右移
	//如果是正数的话，右移不管是逻辑右移还是算数右移左边数都是补0
	//所以要用一个负数来测一下
	//来一个-1
	//*******补充-原码就是这个数用二进制表示的形式
	int a = -1;
	int b = a >> 1;//移动的是二进制位
	//把b打印出来看看
	printf("%d\n",b);  //打印出-1，说明当前的编译器采用的是算数移位
	//右移通常见到的都是算数右移
	return 0;
	//正数的二进制表示有，原码，补码，反码 
	//存到内存中的是补码
	//例如：-1怎么写出他的原、凡、补。
	//因为他是负数，所以最高位先写1，1表示负数的意思
	//最高位表示符号位	
	//10000000000000000000000000000001----原码
	//前面的1表示负数的意思，后面的1表示十进制的一个数字是1，
	//他们两个组合起来就是-1
	//反码
	//符号位不变，其他位按位取反、
	//11111111111111111111111111111110---反码
	//补码就是反码+1
	//11111111111111111111111111111111--补码
	//16进制f换成二进制是1111
	//1111 第一个1是2的3次方是 8
	//第二个1是4，第三个是2，第四个是1
	//8421 = 15
	//****************************
	//当对补码进行移位-算数移位
	//右移补充个1，虽然我丢了个1，又补充了个1，内容还是全1
	//是全1，就是-1的补码，打印的就是-1
}
// >>右移操作符
//1算数右移 右边丢弃，左边补原符号位
//见到的基本上都是算术右移
//2逻辑右移 右边丢弃，左边补0

//整数的二进制表示有：原码 反码 补码
//存储到内存的是补码
```

```c
//左移
#include<stdio.h>
int main(void)
{	
	int a = 5;
	int b = a << 1;
	//5的二进制表示是101
	//0000000000000000000000000000000101
	//经过向左移动左边补0后，当前的数字从5变成了10
	
	return 0;
 
```



### 位操作符

只能作用到整数上去

注意：他们的操作符数必须是整数

&-按位与

```c
#include<stdio.h>
int main(void)
{	
	//&-按位与-按二进制位与
	int a = 3;
	int b = 5;
	int c = a & b;
	//拿到的都是这个数的补码
	//00000000000000000000000000000011
	//00000000000000000000000000000101
	//整型占32个比特位
	//00000000000000000000000000000001
	//有一个为0就是0，两个都是1 才是1 
	printf("%d\n", c);
	return 0;
}
```

|-按位或

```c
#include<stdio.h>
int main(void)
{	
	int a = 3;
	int b = 5;
	int c = a & b;
	//拿到的都是这个数的补码
	//00000000000000000000000000000011
	//00000000000000000000000000000101
	//00000000000000000000000000000111
	//有一个为1就是1，两个都是1 也是1 
	printf("%d\n", c);
	return 0;
}
```

^-按位异或-按二进制位

```c
#include<stdio.h>
int main(void)
{	
	//&-按位与-按二进制位与
	int a = 3;
	int b = 5;
	int c = a & b;
	//拿到的都是这个数的补码
	//00000000000000000000000000000011
	//00000000000000000000000000000101
	//00000000000000000000000000000110
    //对应的二进制如果相同为0，相异为1
    
	printf("%d\n", c);
	return 0;
}
```

一道变态的面试题目

#### 不创建临时变量(第三个变量)实现两个数的交换

```c
不创建临时变量(第三个变量)实现两个数的交换
```

```c
先来一个创建临时变量的
#include<stdio.h>
int main(void)
{	
	int a = 10;
	int b = 20;
	int c = 0;//创建c为临时变量
	printf("交换之前:a=%d,b=%d\n", a, b);
	c = a;
	a = b;
	b = c;
	printf("交换之后:a=%d,b=%d\n", a, b	);
	return 0;
}
```



```c
加减法
    //缺陷
    //当他们特别大的时候,没有超过最大值，但是加在一起的时候超过，必然就会有一些二进位丢失，就还原不出来了，缺陷——可能会溢出
#include<stdio.h>
int main(void)
{
	int a = 10;
	int b = 20;
	printf("交换之前:a=%d,b=%d\n", a, b);
	a = a ^ b;
	b = a ^ b;
	a = a ^ b;
	printf("交换之后:a=%d,b=%d\n", a, b);
	return 0;
}
```



```c
按位异或
    //执行效率不高，可读性比较差
    #include<stdio.h>
int main(void)
{
	int a = 10;
	int b = 20;
	printf("交换之前:a=%d,b=%d\n", a, b);
	//整型在内存中战32个比特位
	//ab分别用二进制表示
	//00000000000000000000000000001010
	//00000000000000000000000000010100
	//进行三次按位异或——相同为0，相异为1
	a = a ^ b;
	//00000000000000000000000000011110——这个所表示的数是30
	//即现在的a变成了30
	//*******现在要进行按位异或的ab分别是
	//00000000000000000000000000011110
	//00000000000000000000000000010100
	b = a ^ b;
	//得到
	//00000000000000000000000000001010——这个所表示的数是10
	//即现在的b变成了10
	//*******现在要进行按位异或的ab分别是
	//00000000000000000000000000011110
	//00000000000000000000000000001010
	a = a ^ b;
	//得到
	//00000000000000000000000000010100——这个所表示的数是20
	//即现在的a变成了20
	printf("交换之后:a=%d,b=%d\n", a, b);
	return 0;
}
```

#### 编写代码实现求一个整数存储在内存中的二进制中1的个数

```c
#include<stdio.h>
int main(void)
{	
	int num = 0;
	int count = 0;
	scanf_s("%d", &num);
	//统计Num的补码中有几个1 
	while(num)
	{	
		if (num % 2 == 1)
			count++;
		num = num / 2;
	}	
	printf("%d\n", count);
	return 0;
}
//代码存在问题，算负数的时候
```



### 赋值操作符

赋值操作符是一个很棒的操作符，你可以让他得到一个你之前不满意的值，也就是说你可以给自己重新赋值，

当一个变量已经有值了，但是你不满意这个值，就给他新一个值，（变量在开始创建的时叫做初始化，当变量已经有一个值得时候，再给他赋值）

可以连续进行赋值，但是不推荐。

连续赋值例子：x = a = b,将b赋给a，再将a赋给x,这样的写法不太容易读懂

而这样写会更加爽朗：a = b;  x = a;并且容易调试

容易混淆：一个=是赋值操作符，两个==这个是判断是否相等

#### 复合赋值符

a = a +2;

a += 2;这个就是复合赋值符



a = a >> 1 ;

a >>= 1

a = a ^1

a ^= 1

### 单目操作符

意思就是只有一个操作数

例如： a + b中的+，它两边有两个操作数，所以他就叫双目操作符

有哪些单目操作符

! 逻辑反操作——真变成假假变成真

```c
int a = 10;
printf("%d\n",!a);逻辑反操作，此时为假 输出为0
    //一般放在if语句中来使用，要根据实际的场景来进行使用
```



### 关系操作符

+正值——正号一般都省略掉了

&取地址——一般都和指针配合使用

```
这个东西与下面的解引用操作符是一对，通常会出现在一起
```



```c
int a = 10;
//&取地址操作符
int* p = &a;//p称为指针变量,int*是p的类型
//*p 解引用操作符
*p
    //通过p里面所存的这个值，来找到它所指向的对象，*p就是它所指向的那个对象a，因为p是指向a的,p里面存了a的地址，所以p是指向a的，现在*p就是a，
    //现在我这样
    *p = 20;//现在a的值变成20了 
return 0;
```

*解引用操作符（间接访问操作符）

sizeof操作数的类型长度（以字节为单位）

```c
int a = 10;   //4
char c = 'r';  //1
char* p = &c;  //  指针的大小要不是4个字节，要不就是8个字节，与你电脑系统格式32位 还是64位有关，
int arr[10] = {0};  // 他是一个数组，里面有十个整型元素，一个整型元素占四个字节，10个整型这里就是      40个字节
//对于数组来说，去掉数组名，剩下的就是他的类型，例如 int crr[40],它叫做crr，去掉他的名字，剩下int[40],这就是他的类型
************
    //可以通过名称来计算大小，也可以通过类型来计算大小 
    注意：当sizeof后面是类型的时候是不可以省略括号的，当后面是名字的时候是可以省略括号的 
//下面sizeof计算的是变量所占内存空间的大小，变量包含数组，单位是字节
printf("%d\n",sizeof(输入你要计算的东西));

```

```c
例题：下面这个代码输出的两个值分别是什么
    #include<stdio.h>
    int main(void)
{								
    short s = 0;
    int a = 10;
    printf("%d\n",sizeof(s = a + 5));//无论a是什么，现在放到了s的=后面，就要按着s的规矩来，一个四个字节的a放到s这里来，也只能存两个字节，因为s为短整型，只有两个字节。
    printf("%d\n",s);//这个地方输出0，并不意味着这个地方就放不下15，而是因为，sizeof这个操作符放的这个表达式是不会真实计算的，也就是说a+5仅仅是个摆设放在上面中s=的后面，
    //给我们的感觉是要把一个整型元素放在一个短整型元素中去，这个地方发生截断，但其实这个地方并不会发生截断，sizeof括号中放的这个表达式是不参与直接运算的，s的并没有发生变化，所以s还是0，输出0。
    return 0;
}
//输出结果为 0 和 2

```

~按位取反

这个位是第二进制位

对有所有位按位取反

```c
#include<stdio.h>
int main(void)
{	
 	int a = 0;
    //a在内存中的二进制位是
    //00000000000000000000000000000000  补码
    //按位取反得到
    原码取反加一能得到补码
        补码减一取反能得到原码
    //11111111111111111111111111111111  补码 
    //11111111111111111111111111111110  反码
    //10000000000000000000000000000001  原码  第一个为符号位
   printf("%d\n",~a);   //打印出来-1
    return 0;
}
```

（类型）——强制类型转换

```c
#include<stdio.h>
int main( void)
{	
	int a = (int)3.15;
	pinrtf("%d\n",a);
	return 0;
}
```

1

![image-20201228160445425](C语言笔记.assets/image-20201228160445425.png)



40 10 4 4	

### 关系操作符

比较大小  > >=  < <=  !=  ==

区别 = 和==

### 逻辑操作符

#### &&逻辑与

逻辑与还具有短路的功能，当式子左边算出为假，那么后面的式子不管什么都不再计算，（后面的不会执行）

其中一个为假， 结果就为假，

两个都为真，结果 为真

```c
#include<stdio.h>
int main(void)
{	
    int a  = 3;
    int b  = 5;
    int c  = a && b:
    //1为真 0为假 a和b有一个为假 就为假
    printf("%d\n",c);
	return 0;
}
```



#### ||逻辑或

一个为真或起来就真，

两个同时为假才为假。

例题：

```c
#include<stdio.h>
int main(void)
{	
	int i = 0, a = 0, b = 2, c = 3, d = 4;
	i = a++ && ++b && d++;
	printf("a = %d b = %d c = %d d = %d ", a, b, c, d);
	return 0;
}
```

![image-20201228161519161](C语言笔记.assets/image-20201228161519161.png)

结果：1 2 3 4

```c
//将上个式子a的值改成1
结果为 2  3  3  5
```



```c
改成逻辑或
#include<stdio.h>
int main(void)
{
	int i = 0, a =1, b = 2, c = 3, d = 4;
	i = a++ || ++b || d++;
    //只要开始为真，整体就为真，后面的不进行运算
	printf("a = %d b = %d c = %d d = %d ", a, b, c, d);
	return 0;
}
```



### 条件操作符（三目操作符）

exp1 ? exp2 ：exp3

 如果表示式1的结果为真，表达式2要算，表达式2的结果整个表达式的结果，

如果表达式1的结果为假，表达式3要算，表达式3的结果是整个表达式的结果。

```C
//如果要实现下面这个代码，转成条件表达式是什么样？
if (a > 5)
	b = 3;
else 
	b = -3; 
	//****************************************************
#include<stdio.h>
int main(void)
{

	int a = 0;
	int b = 0;
	scanf_s("%d",&a);
	b = (a > 5 ? 3 : -3);
	printf("%d", b);
	return 0;
}
```

练习：使用条件表达式得到两个的数较大值

```c
#include<stdio.h>
int main(void)
{		
	int a = 0, b = 0, c = 0;
	printf("请输入两个数：\n");
	scanf_s("%d %d", &a, &b);
	c = (a > b ? a : b);
	printf("较大的数为：%d\n", c);
	return 0;
}
```



### 逗号表达式

exp1, exp2, exp3, ...expN

逗号表达式，就是用逗号隔开多个表达式。

逗号表达式，从左向右依次执行。整个表达式的结果是最后一个表达式的结果。

```c
//代码1
#incldue<stdio.h>
int main(void)
{		
   	int a = 1;
    int b = 2;
    int c = (a >b ,a = b+10,a，b = a +1);
    //a > b这个表达式不产生结果  a不产生结果
    //
	return 0;	
}
```

```c
代码2
    if( a = b +1,c=  a /2, d > 0 );
//真正起到判断作用的是最后一个d>0,d大于0，条件为真，反之条件为假
```



```c
代码3
a = get_val();
count_val(a);
while (a >0)
{
    //这里面上面的代码重复出现了两次，比较啰嗦
    //业务处理
    a = get_val();
    count_val(a);
}
//如果使用逗号表达式，则为
//逻辑一模一样，重复的代码只出现了一次，使代码更加简洁，但有的时候可能不太容易理解。
while(a = get_val(),count_val(a),a > 0)
{
    	//业务处理
}
```



### 下标引用、函数调用和结构成员

1.[]下标引用操作符

操作数:一个数组名+一个索引值

```c
int arr [10];//创建数组
arr[9] = 10;//实用下标引用操作符
[]的两个操作符是arr和9,一个是数组名，一个是索引值
```

2.（）函数调用操作符 接收一个或多个操作数： 第一个操作数是函数名吗，剩余的操作数就是传递给函数的参数。 

```c
#include<stdio.h>
//这里的括号是我们定义函数时候的语法规则
//并不是函数调用操作符
int get_max (int x,int y)
{
    return x >y ?x >:y;
    //这里用了一个三目操作符，复习一下
    //开始是条件，如果x > y成立就返回x,不成立就返回y
}
int main(void)
{	
    int a = 10;
    int b = 20;
    int max = get_max(a , b );
    //调用函数的上面这个()，就是函数调用操作符，
    //函数调用操作符的操作数是多少呢？
//函数名get_max就是操作数，a,b这两个参数也是操作数，
    //这里一共有3个函数操作数
    printf("max = %d\n",max);
	return 0;
}
```

```c
函数调用操作符最少有1个，当一个参数都没有时,就是函数名
```

3.访问一个结构的成员

.结构体.成员名

结构体指针->成员名

```c
//如何描述一个学生来引出概念
#include<stdio.h>
//学生
//用结构体来描述一个学生
//{}里面
//*******************************************
    创建了一个结构体类型，类型名字是下面那个
    //*********************************************
    这里创建好了一个结构体类型，就相当于在盖房子的时候，设计好了图纸，
    图纸画好了只是将房子的大致样子给描述了出来，但是房子还没有盖，
    买完材料，照着图纸盖
    //*********************************************
    向内存申请区域来存放我们相关的信息，
struct stu//定义了一个结构体stu
    //struct stu是一种学生类型  
    //int float是一种类型，
    //这里的struct stu也是一种类型
    //类型是用来创建变量的
{
    //在这里面放的是描述学生的一些相关属性
    char name[20]; 
    age;
    char id[20];
}
int main(void)
{	
    //类型是用来创建变量的
    int  a ;
    //初始化
   //类似于数组
    **************************
        //使用结构体类型，创建了一个结构体变量
        //即创建了一个学生对象s1，并初始化
    struct stu s1 = { "SB"，18,30061128};
    //打印
    printf("%s\n",s1.name);
    printf("%d\n",s1.age);
    printf("%s\n",s1.id);
    //. 操作符用来访问结构体成员
    //结构体变量.成员名
    *******************************************************
    //在内存中开辟了空间，开辟了空间就有起始地址
    struct stu* ps = &s1;//取出s1的地址，存放到ps中,ps就是指针变量
	如何靠拿到的ps来打印sb的信息呢
    printf("%s\n",(*ps).name);
    printf("%d\n",(*ps).age);
    printf("%s\n",(*ps).id);
    //****************************************************
    精简的方式
    printf("%s\n",ps->name);//意思同上//这样写更加直观一些
    //结构体指针->成员名
    return 0;
}
```

学操作符——为了求一个表达式的结果——表达式求值

## 表达式求值

表达式求值的顺序一部分是由操作符的优先级和结合性决定。

同样，有些表达式的操作数在求值的过程中可能需要转化为其他类型。

### 隐式类型转换

就是偷偷的进行类型转换

c的整型算数运算总是至少以缺省整型类型的精度来进行的，

#### 整型提升

为了提升这个精度，表达式中的字符和短整型操作数在使用之前被转换为普通整型,这种转换称为整型提升。

什么时候用到整型提升——可能小于int长度的整型值。

例如

```c
//
char a,b,c;
a = b + c ;
//b和C的值被提升为普通整型，然后再执行加法运算
//加法运算完成之后，结果将被截断，然后再存储于a中
```

```c
//例子
#include<stdio.h>
int main(void)
{	
    char  a = 3;
    //3是一个整数，整型在二级制中占32个比特位
    //00000000000000000000000000000011
    //整数的原码补码反码相同
    //char是字符型变量，存放字符型变量
    //将3放到a里面切，a是一个字节，只能占8个比特位
    //这时候就会发生截断
    //调最小最低位字节的内容 
    //00000011
    
    char b = 127;
    //在二进制中表示为
    //00000000000000000000000000111111
    //同上a
    //这里的b真正存的是
    //01111111
    char c = a + b;
    //a和b如何相加
    //为了提升计算的精度，就要进行整型提升
    //先把它补充成一个整型在二进制中存的位数
    //即32个比特，没有的位置补0
    //00000000000000000000000000000011
    //00000000000000000000000000111111
    //够2就进1
    //结果为
    //00000000000000000000000001000010
    //求完这个结果还要把这个结果放到c里面去，
    //C是一个char类型,只能存8个比特位，继续进行截断，拿到他的8个比特位
    //得到
    //10000010
    //下面要进行打印，打印的类型仍然是个整型
    //这里还要进行整型提升
    printf("%d\n"，c);
    //输出结果为-126，这是为什么呢？
    
    return 0;
}
//原码取反加1到得到反码
```

如何进行整型提升呢？

整型提升是按照变量的数据类型的符号位来提升的

首先看变量是什么类型的（是否为有符号数），高位为该变量的符号位，按照高位提升、 补充，补充成32位（整型），无符号直接前面 补0就行了， 

******************************************************************************

整型提升的意义：
	表达式的整型运算要在CPU的相应运算器件内执行，CPU内整型运算器（ALU）的操作数的字节长度一般就是int的字节长度，同时也是CPU的通用寄存器的长度。

因此，即使两个char类型相同，在cpu执行时实际上也要先转化为 CPU 内整型操作数的标准长度。

通用cpu是难以直接实现两个8比特字节 直接相加运算（虽然机器指令中可能有这种字节相加指令）。所以，表达式中各种长度长度可能小于int长度的整型值，都必须先转换为int或unsign int，然后才能送入cpu去执行运算。

说白了，整型提升就是把其他类型的变量，转化为整型，然后再在程序中计算。

```c
//例题
//下面这个代码的输出结果是什么？
#include<stdio.h>
int main(void)
{	
    char c = 1;
    printf("%u\n",sizeof(c));//1
    printf("%u\n",sizeof(+c));//4
    printf("%u\n",sizeof(!c));//1
    return 0;
    //上面这个代码中,c只要参与运算，就会发生整型提升，表达式c+，就会发生提升，所以sizeof(+c)是4个字节，表达式-c也会发生整型提升，所以sizeof(-c)是4个字节，但是sizeof(c)就是一个字节。
    ***********************************************
    //前面加入的这几个符号是什么意思?
    //
}
```

#### 算数转换

如果某个操作符的各个操作符属于不同的类型，那么除非一个操作数转换为另一个操作数的类型，否则操作就无法进行。下面的层次体系称为寻常算数转换。

下面的操作符从上往下所占空间越来越小

long double

double

float

unsigned long int

long int 

unsigned int 

int 

如果某个操作数的类型在上面这个列表中排名较低，那么首先要转换为另一个操作数的类型后执行运算。

就是，假如说我们从上面随便取两个类型的变量来运算，就要先把小的变量转化为那个较大类型的变量来进行运算。

警告，但是算数转换要合理，要不然会有一些潜在的问题。

```c
//例如
float f = 3.14;
int num = f;
//隐式转换，会有精度丢失
```

### 操作符的属性

复杂的表达式的求值有三个影响的因素

1操作符的优先级

2操作符的结核性

3是否控制求值顺序

两个相邻的操作符限制性哪个？取决有他们的优先级。如果两者的优先级相同，取决于他们的结核性。

操作符的优先级

```C
例子
#incldue<stdio.h>
int main(void)
{	
    int a = 10;
    int b = 20;
    int c = b + a * 3;//*优先级较高，先进行乘再进行加，
    //当相邻的操作符不相同的时候，他们就有自己的优先级
    //优先级高的先算，优先级低的后算
    //若两个操作符相同，那么应该先计算哪个呢？
    //这时候就引出我们要说的结核性
    //从哪边向哪边进行结合，
    //除了优先级和结核性
    //还要看——是否控制求值顺序
    
    return 0;
}
```

优先级较高，先进行乘再进行加，
当相邻的操作符不相同的时候，他们就有自己的优先级
优先级高的先算，优先级低的后算
若两个操作符相同，那么应该先计算哪个呢？
这时候就引出我们要说的结核性
从哪边向哪边进行结合，
除了优先级和结核性
还要看——是否控制求值顺序

不同的求值顺序，得到的值有可能不一样，

计算路径无法确定——有问题表达式

写代码要写不会出现计算路径问题的表达式

别让求值顺序影响代码的运行，这样的代码不好进行运算，容易出现问题。

在调用函数的时候，加入运算符，不知道先调用哪个函数了

无法说明通过这个符号，哪个函数先进行调用。

# 33指针（初阶）

## 1指针是什么

利用地址的这个值，直接找到所需的变量单元，地址指向该变量单元，因此，将地址形象化的称为指针，意思是通过它能找到以他为地址的内存单元。

要讲清楚指针，首先要先了解一下内存。

内存是一块大大的空间，为了更好的使用内存，我们把内存划分为一块一块的小格子，之后再给每个格子编上号。

一个内存单元的大小最好是一个字节。

通过地址就可以找到一个内存单元。

就是说，一个东西存着另一个东西的地址，通过这个地址可以直接找到对应的那个东西的数值，这个东西就是指针。

地址指向该变量单元，将他形象的成为指针。

用来存放地址的变量——指针变量

指针就是地址，地址就是指针。

指针就是一个变量，这个变量中存的是地址，也就是说指针就是地址。

```c
#include<stdio.h>
int main(void)
{	
    int a = 10;//在内存中开辟一块空间
    int p = &a;//这里我们对变量a，取出他的地址，再将a的地址存放到变量p中，p就是一个指针变量。 
    
    return 0;
}
```

32位平台指针大小是4，

## 2指针和指针类型

1指针类型决定了指针进行解引用操作的时候，能够访问空间的大小。

这就使我们未来在给指针进行赋值的时候要选择一个合适(合理)的大小。

2指针类型决定了：指针走一步走多远（指针的步长）

总结：指针类型决定了指针向前或者向后走一步有多大（距离-字节）

（指针的类型决定了，对指针解引用的时候有多大的权限（能够操作几个字节），比如char*的指针解引用就只能访问一个字节，而int* *的指针解引用就能访问4个字节。）

******************************

指针加减整数：

```c
int main(void)
{	
    int a = 0x11223344;
    int* pa = &a;
    char* pc = &a;
    printf("%p\n",pa);
    printf("%p\n",pa+1);
    
    printf("%p\n",pc);
    printf("%p\n",pc+1);
	return 0;
}
//打印结果是什么呢？

```

![image-20210125144919989](C语言笔记.assets/image-20210125144919989.png)

由此可看到，不同类型的指针，所存的地址是一样的。但是在加减整数的时候，同加减一个数，得到的结果是不一样的。

结论：

​		指针类型决定了，指针+1向后面跳了几个字节。

​		即上面的结论——指针的类型决定了指针的步长。就是在加减整数的时候跳多少字节 。不同的类型跳的字节数不同。

​		是什么类型加1 就跳过一个什么，例如是整型类型，加一就跳过一个整型，一个整型占4个字节，所以这时候的步长就是4，跳过了4个字节。加一个字符就跳过1个字节。

​	指针的类型决定了指针向前或向后走一步的有多长（距离）

```c
//将数组中的全部元素，都改成1
#include<stdio.h>
int main(void)
{	
    int arr[10] = { 0 };
    int * p = arr;//数组名首元素地址
    int i = 0;
    for( i = 0; i < 10 ; i++)
    {
        *(p + 1) = 1;
        
    }
	return 0;
}
//如果将上面的p指针类型改成char*
//最后的结果不同，因为不同类型的指针大小不同
********************
    根据你的需求，将地址交给一个合理的指针
```

## 3野指针

野指针就是指针指向的位置是不可知的——随机的，不正确的，没有明确限制的

局部变量不初始化，默认是随机值

在内存中随便找了一个位置放了这个随机值的地址。

```c
#include<stdio.h>
int main(void)
{	
	int a ;//局部变量不初始化，默认是随机值
    int *p ;//局部的指针变量，就被初始化随机值。
    *p = 20;
	return 0;
}
```

```c
一个指针越出了，他所能管理的范围后，他就被成为野指针——指针越界访问
    如下：
#include<stdio.h>
int main(void)
{	
	int arr[10] = { 0};
	int *p = arr;
	int i = 0;
	for(i = 0;i < 12 : i++)
	{
		p++:
	}
	retunr 0;
}
```

```c
指针指向的那块内存空间释放了——(涉及到动态内存开辟)——延迟的问题
int *test()
{
	 int a = 10;  // a 为 局部变量，出这个变量就不存在了，这块内存空间就还给了操作系统
     
	 return &a;
    //这里返回的是a在内存中的地址，让下面的int*p来进行接收，但是这里的a是局部变量。
    //返回的是局部变量的地址，除了这个局部变量所在的大括号，他所占的这个4个字节就还给内存空间了
}
int main(void)
{	
	int *p = test();
	*p = 20;
	return 0;
}
```

如何规避野指针

#### 1指针初始化

当未初始化的指针变量进行解引用操作的时候，就是在内存中随便找了个地方，就把地址存到这里了。

初始化，然后再解引用

#### 2小心指针越界（指针越界访问）

#### 3指针指向空间释放，即设置NULL

（这里在动态访问的时候会详解）

NULL是用来初始化指针的，给指针赋值。（int* p = NULL;）空指针

当指针不用的时候，把他设置为空指针，无法访问他的空间，不能用。（在调试程序窗口验证）

#### 4指针使用之前检查有效性

## 4指针运算

#### 指针加减整数

```c
#include<stdio.h>
int main(void)
{	
    int arr[10] = { 1,2,3,4,5,6,7,8,9,10};
    int i = 0
    int sz = sizeof(arr) / sizeof(arr[0])
    int *p = arr;
    for(i = 0 ; i <sz; i++)
    {	
        printf("%d".*p);
        p = p + 1;
    }
    return 0;
}
```

#### 指针-指针

就是地址-地址

得到的其实是中间的元素个数

相反如果小地址减去大地址，得到的数就是负数。

不同类型的指针无法相减

这两个指针指向的同一块空间

不可预知的，我们也无法知道他的结果

```
#inlcude<stdio.h>
int main(void)
{	
	int arr[10]={1,2,3,4,5,6,7,,9,10};
	&arr[9] - &arr[0];
	printf
	return 0;
}
```

![image-20210205164103822](C语言笔记.assets/image-20210205164103822.png)

C语言标准规定-允许像数组元素的指针与指向数组最后一个元素后面的那个内存位置的指针比较，但是不允许与指向第一个元素之前的那个内存位置的指针比较。

再进行一次理解：（重复了）

就是允许和该指针和该数组最后一个元素后面的那个内存位置的指针进行比较，不能喝第一个前面那个指针内存地址进行比较。

指针的关系运算

&数组名-数组名不是首元素的地址-数组名表示整个数组-数组名-取出的是整个数组的地址（整个数组的地址）

sizeof（arr）-sizeof（数组名）此时此刻的数组名，表示的是整个数组，计算的是整个数组的大小，单位是字节。

除了上面这两个情况——其他位置的数组名代表的都是首元素地址

![image-20210205171046592](C语言笔记.assets/image-20210205171046592.png)

## 5指针和数组

数组可以通过指针来进行访问，但是数组和指针不是一回事，数组可以存放多个相同类型的数组，指针可以存放地址，数组首元素的地址，或者说存放数组任意位置的地址。

他们不是一回事，但是他们两个之间有联系。

## 6二级指针

平时我们写的指针都是一级指针，那么什么是二级指针呢？

```c
int a = 10;
int* pa = &a;
int** = ppa = &pa;//ppa就是二级指针
//3,4,5,6......同理
int ** 第一颗星表示他说指向的对象是个指针
    	第二表示ppa是个指针
```



## 7指针数组

指针数组——本质是个数组——是存放指针的数组gg

数组指针——本质是个指针

字符数组——存放字符，整型数组——存放整型

同理：指针数组——存放指针

arr[数组个数] = {  内容 }

到此为止c语言指针初级到此结束

# 34实用调试技巧

## 1什么是bug

导致计算机不能正常工作的东西叫做Bug

解决bug——调试

历史上的第一个bug——臭虫死在了晶体管上面，导致了该计算机无法正常工作

## 2调试是什么，有多么重要

像现实中做了坏事的心理，顺利而下就是犯罪，逆流而上就是真相。

一个优秀的程序员，是一位优秀的侦探。

猜测代码哪里出错了，但是不知道哪里真正的出错了，这样是不行的。

## 3debug和release的介绍

二者都会产生一个可执行程序

二者的功能可能会有所不同

debug通常称为调试版本，他包含调试信息，并且不做任何优化，便于程序员调试程序。所以文件要较大一点。



release称为发布版本，它往往是进行了各种优化，使的程序在代码大小和运行速度上都是最优的，以便用户很好的使用，无法进行调试，所以文件要较小一点，

## 4调试的基本步骤：

1发现程序错误的存在

2以隔离、消除等方式对错误进行定位

3确定错误产生的原因

5提出纠正错误的解决办法

6对程序错误予以改正，重新测试

7测试完成，解决错误7

****************

1程序员写程序（发现错误代价最小）——交给——2软件测试人员——交给——3用户（可以反馈问题，代价比较大，给用户带来损失）

*********************



## 5windows环境调试介绍

vs快捷键

f5——启动调试，和f9配合使用，单独使用不好用，快速跳到问题可能出现的地方。跳到执行逻辑的下一个断点

f9——设置（切换）断点，在按就消失了，断点——代码执行到断点处就停止下来

f10——逐过程

f11——每次都执行一个语句，但是这个快捷键可以使我们的执行逻辑进入到函数内部（这是最常用的）

shilt+f11——从函数内部跳出来

Shift+f5——停止调试

调试的时候程序当前的信息，自动窗口——打开后，将程序执行至此，显示离得较近的变量，让你来观察，去掉此时此刻不想观察的信息

## 6调试实例

## 7如何写出好（易于调试）的代码

## 8编程常见的错误

# 35结构体

## 1结构体类型的声明

什么是结构（结构体）

结构是一些值的集合，这些值称为成员变量，结构的每个成员可以是不同类型的变量

```c
//描述一个学生的一些数据信息
//名字
//年龄
//电话
//性别
 typedef(给这个类型重新起个名字)struct stu  //struct是结构体关键字   这里的stu叫做结构体标签  Struct stu是我们创建的结构体类型
    //创建类型的时候没有占用内存空间
{	
    	//成员变量 member -list
    char  name[20];
    short age ;
    char tele[12];
    char sexp[5];
    
}s1,s2,s3;//这里的分号不能少，结构体声明是一条语句
//这里的s1s2s3是三个全局的结构体变量


typedef(给这个类型重新起个名字)struct stu  //struct是结构体关键字   这里的stu叫做结构体标签  Struct stu是我们创建的结构体类型
    //创建类型的时候没有占用内存空间
{	
    	//成员变量 member -list
    char  name[20];
    short age ;
    char tele[12];
    char sexp[5];
    
}STU;//现在就可以用STU创建临时变量 ，这个STU是类型，上面的s是变量 ，现在这个STU就可以单独使用了
int mian
{	
    struct stu s;//这个s是局部的结构体变量
    //当你拿这个结构体类型创建一个叫做s的变量，这里就占用了内存空间
    //就像盖房子，光有了图纸，没有具体的房子
    return 0;
}
```

结构成员的类型：结构的成员可以是标量、数组、指针、甚至是其他结构体。

## 2结构体初始化和定义

```c
在创建结构体变量的时候给他一个值，就是结构体初始化，同其他变量的初始化
int main()
{	
	stu s1 = { "张3" , 20 , "1231564215","男"}；//局部变量，这个结构体类似于Python的字典
	return 0;
}
```

```c
struct c
{
	int a;
	char c; 
	char arr[20];
	double b ;
};
struct t 
{
	char ch[10]
	struct s c ;
	char *pc;
};
int main()
{
		//结构体里面放置结构体
	struct t T = {"HEHE","W","HELOW WORLD","3.14",ARR};
    printf("%s\n",t.ch);
    //这里通过.来进行层级区分
	return 0;
}
```



## 3结构体成员访问

结构体.成员 进行访问

## 4结构体传参

函数也可以调用

-->箭头

![image-20210206141542207](C语言笔记.assets/image-20210206141542207.png)

打印结构体数据，在这里封装一个函数

print1和Print2哪个更好？

print1要再内存中再进行一份临时拷贝的形参，无论是速度还是性能都要逊于Print2,

而Pirnt2无论结构体多大最多8个字节（地址），所以Print2的系统开销更小一些。

综上所述，print2更好

****

用更加正规的话说——函数传参的时候，参数是需要压栈的。如果传递一个结构体对象的时候，结构体过大，参数压栈的系统开销会比较大，所以导致性能的下降。

函数传参的时候的压栈是什么呢？

内存在系统中是一块大的空间，里面分为几个空间。

栈区，堆区，静态区（学习计算机语言的时候所讲的）

栈区——局部变量、函数的形式参数、函数调用也开辟空间

堆区——动态内存分配、malloc/free、realloc、calloc、

静态区——全局变量、静态变量

每一次函数调用都要开辟空间

***************

涉及到数据结构的东西：

——————线性数据结构

顺序表——在内存中连续出现

链表——拿一条链子将多个数据串连起来

栈——顶上进顶上出，

队列



结论：结构体传参的时候，要传结构体的地址。

# 

# 36数据的存储

就像是初级阶段类型的深入讲解

## 1数据类型详细介绍

c语言的类型分为两类

1内置类型：语言本身具有的类型

char 

short

long

int 

2自定义类型（构造类型）：

*****

类型的意义：

1使用这个类型开辟内存空间的大小（大小决定了使用范围）

2如何看待内存空间的视角（放置的东西不同，存进去的东西也不同）

****

  整型家族

char

​	unsign char（无符号）  

​	signed  char (有符号)

剩下的同上Char分为有无符号

short

int

long

******

浮点型家族

float单精度浮点型

double双精度浮点型

*******

构造类型（自定义类型）

数组类型

结构体类型Struct

枚举类型enum

联合类型union

*****

指针类型

创建的大小都是4个字节，统一的

目的是为了存放地址

int *p

char *

float *
viod *——无具体类型的指针——空指针

******

空类型 ——void

当你写函数的时候，你不想调用他，void 函数()

通常应用于函数的返回类型，函数的参数，指针类型

如果你明确想不用这个函数传参

就在函数后面的参数部分写一个void







## 2整型在内存中的存储：原码、反码、补码

变量的创建是要在内存中开辟空间的。空间的大小是根据不同的类型而决定的。

下面我们将要了解到数据在开辟内存空间中到底是如何存储的。

```c
#include<stdio.h>
int main(void)
{	
    int a = 20;
    int b = -10;
    return 0;
}
```

反码、补码、原码的概念

计算机中有符号数（指的是整型有符号数字）有三种表示方法，即原码、补码、反码

三种表示方法均有符号位和数值位两部分，符号位都是用0表示正，用1表示负，而数值为三种表示方法各不相同。

（无符号数也可以说有着三种表示形式，只不过这三种表示在表示数字的时候是相同的）

（有符号数又分为正负数，其中的正数他的原凡补码也都是相同的，负数的原反补才是不相同的）

*********

原码：直接将二级制按照正负数的形式翻译成二进制就可以。

反码：将原码的符号位不变，其他位一次按位取反就可以得到了。

补码：反码+1得到补码

*****

对于整型来说：数据存放在内存中其实存放的就是二进制序列的补码

补码的出现让计算机中的加法减法按照统一的方法处理。

*****

复习：

整数分为有符号数和无符号数

有符号数-正数-原反补相同

​				-负数原反补不同-都要进行计算

无符号数-不区分正负-原反补相同

*****



## 3大小端字节序介绍及判断

十六进制的数字在内存中是倒着存储的，这里引入大小端的概念

什么是大端小端？
大端 （存储）模式 ，是指数据的低位保存在内存的高地址中，而数据的高位，保存在内存的低地址中

小端（存储）模式，是指数据的低位保存在内存的低地址中，而数据的高位，保存在内存的高地址中。

大端字节序存储模式

小端字节序存储模式

*****

```c
//设计一个小程序判断当前机器的字节序
#include<stdio.h>
int main(void)
{	
	//写一段代码告诉我们当前机器的字节序是什么
	//拿出四个字节中的第一个内容是什么
	//假设a里面的是20
	//推测如果是小端和大端分别是什么
	//拿出的第一个字节是什么 14 or 00 ?
	//所以说这四个字节我们就看第一个字节是什么
	int a = 1;
	//那么如何拿到第一个字节的内容呢?
	//这里提到，指针的类型规定了，指针解引用操作一下能访问几个字节
	//Char类型的指针解引用向后就访问一个字节的内容，拿到这一个字节的内容再判断它是什么
	char* p = ( char* ) &a;
	//强制类型转换，a的地址取出来，值不变，类型变成char*,放到p里面去
	if (p == 1)
	{
		printf("小端\n");
	}
	else
	{
		printf("大端\n");
	}

	return 0;
}

//函数方法
#include<stdio.h>
//函数的作用很干净
int check_sys()
{
	int a = 1;
	char* p = (char*)&a;
	if (*p == 1)
		//	return 1;
		//else
		//	return 0;
		//简化
		return *(char*)&a;
	//直接对char*的地址进行解引用
}
int main(void)
{	
	//写一段代码告诉我们当前机器的字节序是什么

	if (ret == 1)
	{
		printf("小端\n");
	}
	else
	{
		printf("大端\n");
	}
	return 0;
}
```

复习：指针类型的意义？

1指针类型决定了，指针解引用操作能访问几个字节

2指针类型决定了指针加1，或者减1操作时到底加的是几个字节

```c
//（1）以下代码输出什么？
#include<stdio.h>
int main(void) 
{
	char a = -1;
	signed char b = -1;
	unsigned char c = -1;
	printf("a = %d , b = %d , c= %d", a, b, c);
	return 0;
	//输出结果为a = -1 , b = -1 , c= 255
	//为什么会有一个255呢?
	//默认有符号位的所以前两个一样
	//-1的补码全是1
	//char类型 8个比特位
	//11111111
	//打印出整型来，这里涉及到整型提升
	//unsigned char无符号数整型提升
	//高位补0
	//高位是0——是正数
	//正数的原凡补码相同——再将二进制转换为十进制的数
}
```

******





```c
//以下代码输出什么？  (2)
#include<stdio.h>
int main(void)
{
	char a = -128;
	printf("%u\n", a);
	//%u表示数据按十进制无符号数字
	return 0;
	//10000000  二进制序列表示
	//00000000000000000000000010000000 原
	//11111111111111111111111101111111 反
	//11111111111111111111111110000000 补
	//存到a里面，8哥比特位
	//1000000
	//打印整数-整型提升
	//11111111111111111111111110000000
	//无符号 原补反相同
	//得到结果数字
}
```



char-signed char 

​	   -unsigned char 1个字节 8个比特位



有符号的char的范围是 -128————127

无符号的char的范围是0 - 255

```c
按照补码的形式进行运算，最后格式化成为有符号整数
int i = -20;
unsigned int j = 10;
printf("%d"，i + j);
//结果是  -10
```

```c
int main(void)
{
    unsigned int;
    for(i = 9 ;  i >= 0 ; i --)
    {
        printf("%u\n",i);
    }
}
//结果：死循环
当-1被认为成无符号数的时候，他就是无穷大的
```

```c
int main(void)
{
	char a[1000];
	for( i = 0; i < 1000; i++)
	{
		a[i] = -1 -i;
	}
	printf("%d".strlen(a));
	return 0;
}
//结果是：255
```

```c
#include<stdio.h>
int main(void)
{
	for (i = 0; i <= 255 ; i ++)
	{
		printf("hello world\n");
	}
	return 0;
}
//结果是：死循环(永远成立)
```

******

## 4浮点型在内存中的存储解析

（本片段模糊）

常见的浮点数：3.14159 1E10 

浮点数家族包括：float double long double 类型。浮点数的表示范围：floar.h中定义浮点数存储;



![image-20210228112527344](C语言笔记.assets/image-20210228112527344.png)

![image-20210228120100068](C语言笔记.assets/image-20210228120100068.png)

当你要存储一个二进制序列的时候，先把他用二进制表示



IEEE754规定：对于32位的浮点数，最高的1位是符号位s，接着的8位是质数E，剩下的23位为有效数字M

**E全为0**

这时，浮点数的指数E等于1-127（或者1-1023）即为真实值，有效数字M不再加上第一位的1，而是还原为0.xxxxxx的小数。这样做是为了表示-+0,以及接近于0的很小的数字。

**E全为0**

这时，如果有有效数字M全为0，表示-+无穷大（正负取决于符号位s）

# 37指针（详解）

指针的进阶

 指针知识回顾

1指针就是个变量，用来存放地址，地址唯一标识一块内存空间

2指针的大小是固定的4/8个字节（32位平台/64位平台）

3指针是有类型，指针的类型决定了指针的+-整数的步长，指针解引用操作时候的权限

4指针的运算

由此我们进入指针的进阶

## 1字符指针

在指针的类型中，我们知道有一种指针类型为字符指针char*                   

```c
int main(void)
{	
	char arr[] = "abcdef";
    char *pc = arr;
    printf("%s\n",arr);
    printf("%s\n",pc);
	return 0;
}
//输出都是abcdef
```

​                                      

```c
int main(void)
{	
	char* p = "abcdef";
	return 0;
    printf("%s\n",p);
    //输出abcdef
}
//什么意思 abcdef是一个常量字符串，其实就是把a的地址赋给了p
```

​                  

```c
int main(void)
{	
	char arr1[] = "abcdef";
	char arr2[] = "acbdef";
	char* p1 = "abcdef";
	char* p2 = "abcdef";
	if( arr1 = arr2 )
	{
		printf("hehe\n");	
	}
	else
	{
		printf("haha\n");
	}
	return 0;
}
//输出结果：haha
arr1 2 是两个不同的数组，数组名是首元素的地址，这两个地址在内存不同的空间
```





## 2指针数组

指针数组其实是数组

```c
int main(void)
{	
//什么数组就存放什么
	int arr[10] = {0};//整型数组
	char ch[5] = { 0 }; //字符数组
    int* parr[4];//存放整型指针的数组  - 指针数组
    char* pch[5] ; //存放字符型指针的数组 - 指针数组
	return 0;
}
```

```
int main(void)
{	
	int a = 10;
	int b = 20;
	int c = 30;
	int d = 40;
	int* arr[4] = {&a,&b,&c,&d};
	for(i = 0 ; i < 4 ; i ++)
	{
		 printf("%d"，*(arr[i]));
	}
	return 0;
}
//结果输出为10 20 30 40 
```



## 3指针数组

## 4数组传参和指针传参

## 5函数指针

## 6函数指针数组

## 7指向函数指针数组的指针

## 8回调函数

## 9函数和数组面试题的解析