---
title: JAVA基础知识总结
date: 2021-04-27 16:33:32
tags:
	- -计算机
	- -笔记
	- -JAVA
categories:
	- JAVA
cover: /img/java.png
---

# 1.JAVA基础

## 1. Hello world

**psvm自动生成方法**

**sout自动生成system out **

```java
public class Hello {
    public static void main(String[] args) {
        System.out.println("hello sb!");
    }
}

```

## 2. 注释

**单行注释：**//

**多行注释：**  /**/

**文档注释：**JavaDoc /** * * * */(用处不大)

## 3.标识符和关键字

**Java所有的组成部分都需要名字。类名、变量名以及方法名都被称为标识符。**

![image-20210412205124735](/images/JAVA基础.assets/image-20210412205124735.png)

- 所有的字标识符都应该以字母(A-Z,a-z)、美元符($)、或者下划线(_)开始。
- 首字符之后可以是(A-Z,a-z)、美元符($)、下划线(_)或数字的任何字符组合
- **不能使用关键字作为变量名或方法名**
- 标识符是大小敏感的(**区分大小写**)
- 可以使用中文命名，但是一般不建议，也不建议使用拼音，sb

## 4.数据类型

**强类型语言：**要求变量的使用要严格符合规定，所有变量必须先定义之后才能使用。

(安全、严谨、避免很多错误)

**弱类型语言：**随便定义

**Java的数据类型分为两大类：**

- 基本类型 -数值类型-整数类型-byte-short-int-long

  ​									-浮点类型-float-double

  ​									-字符类型-char

  ​				 -boolean类型//默认true（if）

- 引用类型 - 类

  ​				 -接口

  ​                 -数组

**数据类型+变量名+值     可以使用逗号隔开来声明多个同类型的变量。**

**转义字符：**

**变量：**就是可以变化的量

​			Java是一种强类型语言，每个变量都必须声明其类型

​			Java变量程序中最基本的存储单元，其要素包括变量名，变量类型和作用域。

****

## 5.类型转换

由于Java是强类型语言，所以要进行有些运算的时候，需要用到类型转换。

**运算中，不同类型的数据先转化为同一类型，然后进行运算。**

- 强制类型转换

**从高-低**

**(要转换的类型)变量名**

```java
public class Hello {
    public static void main(String[] args) {
        int i = 128;
        byte b = (byte) i;
        System.out.println(i);
        System.out.println(b);
    }
}
```



- 自动类型转换
- **从低-高**

```java
 int i = 128;
 double b =  i;
```

**注意：**

1. 不能对bool类型进行转换。
2. 不能把对象的类型转换为不相干的类型。
3. 高容量转化为低容量的时候，强制类型转换。
4. 转化的时候可能存在内存溢出，或者精度问题。

精度问题

```java
    System.out.println(18.3);//18.7
    System.out.println((int)-45.89f);//45
```

溢出问题

操作比较大的数的时候

```java
int year = 12;	
int money = 10_0000_0000;
long total = money*year;
//得到的结果溢出，默认是int，在转换之前就出现问题了。
long total2 = money*(long(year));
```

## 6.变量

**什么是变量：**就是可以变化的量。

Java是一种强类型语言，每个变量都必须声明其类型。

Java变量是程序中最基本的存储单元，其要素包括变量名，变量类型和作用域。

**数据类型  变量名 =  值**

**注意：**

- 每个变量都有类型，类型可以是基本的类型，也可以是引用类型。
- 变量名必须是合法的标识符。
- 变量声明是一条完整的语句，因此每个声明都必须以分号结束。

### 变量作用域

```java
public class sb{
    static int allClicks = 0; //类变量
    String str = "hello sb"; //实例变量
    public void method(){
        int i = 0;//局部变量
    }
}
```

```java
public class sb{
    //类变量
    static double salary = 2500;//2500默认是int类型 自动转换成了double
    //属性:简单理解为变量
    //实例变量：从属于对象,如果不进行初始化，这个数值类型的默认值
    //布尔值：默认是false
    //除了基本类型：其余默认值都是null
    String name;
    int age;
    //mian方法
    public static void main(String[] args){
        //局部变量:必须声明和初始化值
        int  i = 23;
        System.out.println(i); 
        sb sb1 = new sb1();
        System.out println(sb1.age);
        //类变量static 
    }
    //其他方法
    public void add(){
        
    }
}
```

### 变量的命名规范

​	![image-20210413204212919](/images/JAVA基础.assets/image-20210413204212919.png)

##  7.常量

常量：初始化之后不能变动的值。

**可以理解为一个特殊的变量。**

**final** 常量名 = 值；(**final是个修饰符，不区分前后。**)

常量名一般使用大写字符。

## 8.运算符

![image-20210413204342365](/images/JAVA基础.assets/image-20210413204342365.png)

**()括号的优先级高**

**关系运算符的结果是: 正确 错误  布尔值 **

```java
//自增++ 自减--
int a = 3;
int b = a++;//执行完这段代码之后先将3赋值给b，然后再自增。
int c = ++a;//执行这行代码前，先给a自增然后再给c赋值。
```

```java
//很多运算我们会使用工具类运算double Pow = Math.pow(3,2);System.out.println(Pow);//9.0
```

```java
&&——逻辑与 两个都为真 结果为真||——逻辑或 其中一个为 结果为真！——取反 真变假 假变真
```

**位运算——看二进制位**

&、|、类似于上面的逻辑操作符

^相同为0 不相同为1

~按位取反 

左移<< ——相当于把数字乘2、

右移 >>——相当于把数字除2、

**字符串连接符**

在+号后出现string类型，就会把操作数转化为字符串然后再连接。

在后面的就会正常进行运算。

```java
public class Hello {    public static void main(String[] args) {        int a = 10;        int b = 20;        System.out.println(" "+a+b);        System.out.println(a+b+" ");        //输出 1020		//    30     }    }
```

**三元运算符**

x?a:b

x为真 结果为a

x为假 结果为b

```java
public class Hello {    public static void main(String[] args) {       int score = 20;       String type = score > 60?"及格":"不及格";        System.out.println(type);    }    }
```

## 9.包机制

- 为了更好的组织类，JAVA提供了包机制，用于区别类名的命名空间。

(基本就是新建一个文件夹将两个名称相同的文件分开放，**包的本质就是一个文件夹**。)

- 包机制的语法格式为：

  ```java
  package pk1[.pk2[.pk3...]];
  ```

- 一般利用公司域名倒置作为包名(www.baidu.com - com.baidu.www)

- 为了能够使用某一个包的成员，我们需要在Java程序明确导入该包。使用import语句可以完成此功能。

  ```java
  import package1[.package2...].(classname|*);
  ```

  # 10.JavaDoc

  是用来生成自己API文档的。

  **参数信息：**

  ![image-20210414210704599](/images/JAVA基础.assets/image-20210414210704599.png)

```java
package sb;/*** @author  zhaoyuxuan* @version  1.0 * @since  1.8 * * */public class DocJava {    String name;    /**     * @author zhaoyuxuan      * @param name     * @return     * @throws  Exception     */    public String  test(String  name) throws Exception{        return  name;    }}
```

IDEA、命令行都可以生成javadoc文件。

#  2.JAVA流程控制

## 1.用户交互Scanner

**Scanner获取用户的输入**

**基本语法：**

```java
Scanner s = new Scanner(System.in);
```

通过next()和nextLine()方法获取输入的字符串，在读取前我们一般需要视同hasNext() 与hasNextLine()判断是否还有输入的数据。

```java
import java.util.Scanner;public class demo1 {    public static void main(String[] args)    {        //创建一个扫描器对象        Scanner scanner = new Scanner(System.in);        System.out.println("使用next的方式接收：");        //判断用户有没有输入字符串        if(scanner.hasNext()){            //使用next方式接收            String str = scanner.next();            System.out.println("输出的内容为："+str);        }        //凡是IO流的类如果不关闭会一直占用资源，要养成良好的习惯用完就关掉        scanner.close();    }}
```

**next():**

- 一定要读取到有效字符后才可以结束输入。
- 对输入有效字符之前遇到的空白，next()方法会自动将其去掉。
- 只有输入有效字符后才将其后面输入的空白作为分隔符或者结束符。
- next()不能得到带有空格的字符串。

**nextLine():**

- 以Enter为结束符，也就是说nextLine()方法返回的是输入回车之前的所有字符。
- 可以获得空白。

```java
import java.util.Scanner;public class demo2 {    public static void main(String[] args) {        Scanner scanner = new Scanner(System.in);        //从键盘接收数据        int i = 0 ;        float f = 0.0f;        System.out.println("请输入整数：");        if(scanner.hasNextInt())        {            i= scanner.nextInt();            System.out.println("整数数据："+i);        }        else{            System.out.println("输入的不是整数数据");        }        System.out.println("请输入小数数据：");        if(scanner.hasNextFloat())        {            f = scanner.nextInt();            System.out.println("小数数据："+f);        }        else{            System.out.println("输入的不是小数数据");        }    }}
```

```java
import  java.util.Scanner;public class demo3 {    public static void main(String[] args) {        //我们可以输入多个数字，并求其总和 每输入一个数字用回车确认，用过输入非数字来结束        //输入并输出执行结果。        Scanner scanner = new Scanner(System.in);        //和        double sum = 0;        //计算输入了多少个数字        int m = 0;        while(scanner.hasNextDouble())        {            double x = scanner.nextDouble();            m = m  + 1;            sum = sum +x;            System.out.println("你输入了第"+m+"个数据当前结果sum"+sum);        }        System.out.println(m + "个数字的总和是："+ sum);        System.out.println(m + "个数字平均数是："+ (sum/m));        scanner.close();    }}
```

## 2.顺序结构

- JAVA的基本结构就是顺序结构，除非特别指明，否则就按照顺序一句一句执行。
- 顺序结构是最简单的算法结构。
- 语句和语句之间，框与框之间是按从上到下的顺序进行的，它是由若干个依次执行的处理步骤组成的， **它是任何一种算法都离不开的一种基本算法结构。**

## 3.选择结构

### 1.if单选择结构

- 判断是否可行，然后再去执行
- 语法：

```java
if(布尔表达式){	//如果布尔表达式为True将执行的语句}
```

```java
package struct;import  java.util.Scanner;public class demo1 {    public static void main(String[] args) {        Scanner scanner  = new Scanner(System.in);        System.out.println("请输入内容：");        String s = scanner.nextLine();        if(s.equals("Hello"))        {            System.out.println(s);        }        System.out.println("sb");        scanner.close();    }}
```

### 2.if双选择结构

- if-else
- 语法

```java
if(布尔表达式){	//如果布尔表达式的值为true}else{    //如果布尔表达式的值为false}
```

```java
package struct;import java.util.Scanner;public class if_else {    public static void main(String[] args) {        System.out.println("请输入你的成绩");        Scanner scanner = new Scanner(System.in);        //大于等于60分及格，否则不及格        int score = scanner.nextInt();        if(score >= 60)        {            System.out.println("及格");        }        else        {            System.out.println("不及格");        }    }}
```

### 3.if多选择语句

- if-else if - else - if 
- 语法 ......

### 4.嵌套的if语句

......

```java
if(){    if()}
```

### 5.Switch多选择结构

- switch case
- switch case语句判断一个变量与一系列值中的某个值是否相等，每个值称为一个分支
- 语法

```java
switch(expression){    case value:        //语句        break;    case value:        //语句        break;    ......    default:       // }
```

**switch语句中的变量类型可以是：**

- byte、short、int或者char。
- 从Java SE 7开始。
- switch 支持字符串String类型了
- 同时case标签必须为字符串常量或者字面量。

```java
package struct;public class Switch {    public static void main(String[] args) {        char grade = 'D';        switch(grade)        {            case 'A':                System.out.println("优秀");                break;            case 'B':                System.out.println("良好");                break;            default:                System.out.println("sb");        }    }}
```

**（不加break会发生case穿透现象。）**

```java
//JDK7的新特性，表达式的结果可以是字符串//字符的本质还是数字//反编译//java——class(字节码文件)//——反编译(IDEA)//每一个对象都有自己的一个hashcodepackage struct;public class Switch2 {    public static void main(String[] args) {        String name = "sb";        switch (name)        {            case "sb":                System.out.println("大sb");                break;            default:                System.out.println(123);        }    }}
```

**反编译**

**看原码**

![image-20210415170423339](/images/JAVA基础.assets/image-20210415170423339.png)

## 4.循环结构

### 1.while循环

- while是最基本的循环

```java
while(布尔表达式){    //循环内容}
```

- 只要布尔表达式为true，循环就会一直执行下去。
- 我们大多数情况是会让循环停下来的，我们需要让一个表达式失效的方式来结束循环。
- 少部分情况需要循环一直执行，比如服务器的请求响应监听等。
- 循环条件为true就会造成无限循环【死循环】，我们正常的业务编程中应该尽量避免死循环。会影响程序性能或者造成程序卡死崩溃。

```java
package struct;public class While01 {    public static void main(String[] args) {        //输出1~100        int num = 1;        while(num <101)        {            System.out.println(num);            num++;        }    }}
```

```java
package struct;public class Whiledemo2 {    public static void main(String[] args) {     //计算1+。。。。100        int sum = 0;        int num = 1;        while(num <=100)        {            sum += num;            num ++;        }        System.out.println(sum);    }}
```

### 2.do-while循环

- 对于while语句而言，如果不满足条件，就不能进入循环。但有时候我们需要即使不满足条件，也至少执行一次。
- do...while循环和while循环相似，不同的是，do..while循环至少会执行一次

```java
do{    //语句}while(布尔表达式)
```

- **while和do-while的区别**
  - while先判断在执行，do-while先执行后判断。
  - do-while总是保证循环体被会至少被执行一次，这是他们的主要差别。

```java
package struct;public class Dowhile {    public static void main(String[] args) {        int num = 1;        int sum = 0;        do{            sum += num;            num ++;        }while(num <= 100);        System.out.println(sum);    }}
```

**区别**

```java
package struct;public class Dowhile {    public static void main(String[] args) {         int a = 0;         while(a < 0)         {             System.out.println(a);             a++;         }        System.out.println("``````````````````````````");         do{             System.out.println(a);             a++;         }while (a< 0);    }}
```

### 3.for循环

- for循环是一种支持迭代的一种通用结构，是最有效、最灵活的循环结构。
- for循环的执行次数是在执行之前就确定的。
- 语法
- IDEA快捷键100.for

```java
for(初始化;布尔表达式;更新){	//代码语句}
```

```java
package struct;public class FOr {    public static void main(String[] args) {         for(int i = 1;i <= 100; i++)         {             System.out.println(i);         }    }}
```

```java
package struct;public class FOr {    public static void main(String[] args) {        int oddSum = 0;        int evenSum = 0;        for(int i = 0; i <= 100; i++)        {            if(i%2 != 0)            {                evenSum += i;            }            else            {                oddSum += i;            }        }        System.out.println("偶数的和"+evenSum);        System.out.println("奇数的和"+oddSum);    }}
```

```java
package struct;public class FOR2 {    //练习2，输入1~1000之间的能被5整除的数，并且每行输出3个    public static void main(String[] args) {        for (int i = 1; i < 1000; i++) {            if(i % 5 == 0)            {                System.out.print(i+"\t");            }            if(i %(3*5) == 0)            {                //都可以System.out.println();                System.out.println("\n");            }        }    }}
```

**println(自动换行)和print**

```JAVA
 //九九乘法表package struct;public class Chengfa {    public static void main(String[] args) {        for (int j = 0; j <= 9; j++) {            for(int i =1; i <=j ; i++)            {                System.out.print(j+"*"+i+"="+(j*i) + "\t");            }            System.out.println("");//换行        }    }}
```

**增强for——主要用于数组或集合**

```java
package struct;public class Chengfa {    public static void main(String[] args) {        int[] numbers = {10,20,30,40,50};        //增强型for        for(int x:numbers)        {            System.out.println(x);        }        System.out.println("123456789");        //或者        for(int i = 0; i < 5 ; i++)        {            System.out.println(numbers[i]);        }    }}
```

### 4.break、continue、goto

- break语句在任何循环语句的主题部分，均可用break控制循环的流程。break用于强行退出循环，不执行循环中剩余的语句。(break语句也在switch语句中使用)

- continue语句用在循环语句体中，用于终止某次循环过程，即跳过循环体尚未执行的语句，接着进行下一次是否执行循环的判定。

```java
package struct;public class Break {    public static void main(String[] args) {        int i = 0;        while(i<100)        {            if(i == 30)            {                break;            }            System.out.println(i);            i++;        }    }}////////////////////////////////package struct;public class Break {    public static void main(String[] args) {        int i = 0;        while(i<10)        {            i++;            if(i == 3)            {               continue;            }            System.out.println(i);        }    }}
```

- 关于goto关键字

(JAVA没有)但是有标签

## 5.流程控制练习

**打印三角形5行**

```java
package struct;public class Sanjiao {    public static void main(String[] args) {        for(int i = 1; i <= 5; i++)        {            for(int j = 5 ; j >= i; j--)            {                System.out.print(" ");            }            for(int j = 1 ; j <= i; j++)            {                System.out.print("+");            }            for(int j = 1 ; j < i; j++)            {                System.out.print("+");            }            System.out.println("");        }    }}
```

# 3.JAVA方法

## 1.什么是方法

**就是其他语言的函数。**

- JAVA方法是语句的集合，它们在一起执行一个功能。
  - 方法是解决一类问题的步骤的有序组合。
  - 方法包含于类或对象中。
  - 方法在程序中被创建，在其他地方被引用。
- 设计方法的原则：方法的本意是功能快，就是实现某个功能的语句块的集合。我们设计方法的时候，最好保持方法的原子性，就是一个方法只完成一个功能，这样有利于我们后期的扩展。

```java
package Method;public class demo1 {    //main方法    public static void main(String[] args) {        int sum = add(1,2);        System.out.println(sum);    }    //加法    public static int add(int a,int b)    {        return a+b;    }}
```

## 2.方法的定义

- 结构

```java
修饰符 返回值类型  方法名(参数类型 参数名){    \\\    方法体        \\\    return 返回值；}
```

- 方法包含一个方法头和方法体

  - 修饰符：修饰符，这是可选的，告诉编译器如何调用方法。定义了该方法的访问类型。
  - 返回值：方法可能会有返回值。returnValueType是方法返回值的数据类型。有些方法执行所需的操作，但没有返回值。这种情况下，returnValueType是关键字void。
  - 方法名：是方法的实际名称。方法和参数表共同构成方法签名。
  - 参数类型：参数像一个占位符。放方法被调用的时 ，传递值给参数。这个值被称为实参或变量。参数列表是指方法的参数类型、顺序和参数的个数。参数是可选的，方法可以不包含任何参数。
    - 形式参数：在方法被调用的时用于接收外界输入的数据。
    - 实参：调用方法时实际传给方法的数据。
  - 方法体：方法体包含具体的语句，定义该方法的功能

  **return可以终止方法。**

  ```java
  package Method;public class demo1 {    public static void main(String[] args) {        int result = Max(10,10);        System.out.println(result);    }    public static int Max(int num1,int num2)    {        int max = 0;        if(num1 == num2)        {            System.out.println("num1 = num2");            return 0;        }        if(num1 > num2)        {            max = num1;        }        else        {            max = num2;        }        return max;    }}
  ```

  

## 3.方法的重载

- 重载就是在同一个类中，有相同的函数名称，但形参不同的函数。
- 方法重载的规则：
  - 方法名必须相同。
  - 参数列表必须不同(个数不同、或类型不同、参数排列顺序不同等)。
  - 方法的返回类型可以相同也可以不相同。
  - 仅仅返回类型不同不足以成为方法的重载。
- 实现理论：
  - 方法名称相同时，编译器会根据调用方法的参数个数、参数类型等去逐个匹配，以选择对应的方法，如果匹配失败，则编译器报错。

```java
package Method;public class demo1 {    public static void main(String[] args) {        double result = Max(10.0,20.0);        System.out.println(result);    }    public static int Max(int num1,int num2)    {        int max = 0;        if(num1 == num2)        {            System.out.println("num1 = num2");            return 0;        }        if(num1 > num2)        {            max = num1;        }        else        {            max = num2;        }        return max;    }    public static double Max(double num1,double num2)    {        double max = 0;        if(num1 == num2)        {            System.out.println("num1 = num2");            return 0;        }        if(num1 > num2)        {            max = num1;        }        else        {            max = num2;        }        return max;    }}
```

## 4.命令行传参

有时你希望运行一个程序的时候再传递给它消息。这要靠传递命令行参数给main()函数实现。

```java
package Method;public class demo2 {    public static void main(String[] args) {        //args.Length数组长度        for(int i = 0;i <args.length;i++)        {            System.out.println("args["+1+"]:"+args[i]);        }    }}
```

## 5.可变参数

- 从JKD1.5开始，JAVA支持传递同类型的可变参数给一个方法。
- 在方法声明中，在指定参数类型后面加一个省略号(...)。
- 一个方法只能指定一个可变参数，它必须是方法的最后一个参数。任何普通的参数必须在他前面声明。

```java
package Method;public class Kbian {    public static void main(String[] args) {        //这个东西的本质就是即将要讲的数组        Kbian kbian = new Kbian();        kbian.test(1,23,4,5,3,2);    }    public void test(int...i)    {        System.out.println(i[0]);        System.out.println(i[1]);        System.out.println(i[2]);        System.out.println(i[3]);        System.out.println(i[4]);        System.out.println(i[5]);    }}
```

```java
package Method;public class Diaoyongkebian {    public static void main(String[] args) {        //调用可变参数方法        printMax(34, 2, 5, 4, 8, 1, 5, 6, 55, 45);        printMax(new double[]{1, 2, 3});    }    public static void printMax(double... numbers) {        if (numbers.length == 0) {            System.out.println("这里什么也没有");            return;        }        double result = 0;        for (int i = 0; i < numbers.length; i++) {            if (numbers[i] > result) {                result = numbers[i];            }        }        System.out.println("最大的数是"+result);    }}
```

## 6.递归

- 递归就是A方法调用A方法，自己调用自己。
- 利用递归可以用简单的程序来解决一些复杂的问题。它通常把一个大型复杂的问题层层转化为一个与原问题相似规模较小的问题来求解，递归策略只需要少量的程序就可描述出解题过程所需要的多次重复计算，大大地减少了程序的代码量。递归的能力在于用有限的语句来定义对象的无限集合。
- 递归结构包括两个部分
  - 递归头:什么时候不调用自身方法 。如果没有头，将陷入死循环。
  - 递归体:什么时候需要调用自身方法。
- 注意：
  - 对于一些嵌套比较深的，递归就有些力不从心了，依次压在栈上面，物理上会造成内存崩溃。
  - 一般小计算我们可以用一些递归，大计算还是用一些其他的算法吧。

```java
//计算阶乘//依次类推，就是将这个的结果传递给下一个，学了C语言之后，在学JAVA的，会感觉简单很多，很容易理解的。package Method;public class Self {    public static void main(String[] args) {        System.out.println(f(3));    }    public static int f(int n)    {        if(n == 1) {            return 1;        }        else        {            return n*f(n-1);        }    }}
```

# 4.数组

（基础部分的最后一个东西）

（JAVA的学习，说白了就是学习一个又一个的类。）

## 1.什么是数组

- 数组是相同类型数据的有序集合。
- 数组描述的是相同类型的若干个数据，按照一定的先后次序排列组合而成。
- 其中，每一个数据称作一个数组元素，每个数组元素可以通过一个下标来访问它们。

## 2.数组的声明创建

- 首先必须声明数组变量，才能在程序中使用数组。

```java
dataType[] arrayRefVar;//首选的方法或者dataType[] arrayRefVar[];//效果相同，但不是首选方法//这个是C和C++的写法，早些年为了上程序员更好的熟悉java。
```

- JAVA语言使用new操作符来创建数组。

```java
dataType[] arrayRefVar = new dataType[arraySize];//定义了一个什么类型的数组，就new一个什么类型的数组。
```

- 数组元素是通过索引访问的，数组索引从0开始。
- 获取数组长度:**arrays.length**

```java
package Array;public class demo01 {    public static void main(String[] args) {        //变量的类型 变量的名字 = 变量的值；        //数组类型；        int[] nums ;//定义        nums = new int[10];//这里面可以存放10个int类型的数字。        //给数组中的元素赋值        nums[0] = 1;        nums[1] = 2;        nums[2] = 3;        nums[3] = 4;        nums[4] = 5;        nums[5] = 6;        nums[6] = 7;        nums[7] = 8;        nums[8] = 9;        nums[9] = 10;       //计算所有元素的和        int sum =0;        for(int i= 0 ; i < nums.length ; i++)        {            sum +=  nums[i];        }        System.out.println("数组nums的和是"+sum);    }}
```

## 3.内存分析

- JAVA内存分析：
  - 堆：
    - 可以存放new的对象和数组
    - 可以被所有的线程共享，不会存放别的对象引用。
  - 栈：
    - 存放基本变量类型(会包含这个基本类型的具体数值)
    - 引用对象的变量(会存放这个引用在堆里面的具体地址)
  - 方法区：
    - 可以被所有线程共享
    - 包含了所有的class和static变量

## 4.初始化

- 静态初始化

```java
int[] a = {1,2,3};Man[] mans = {new Man(1,1),new Man(2,2));
```

- 动态初始化

```java
int[] a = new int[2];a[0] = 1;a[1] = 2;
```

- 数组的默认初始化
  - 数组是引用类型，它的元素相当于类的实例变量，因此数组一经分配空间，其中的每个元素也被按照实例变量同样的方式被隐式初始化。

## 5.数组特点

- 数组的长度时确定的。数组一但被创建，它的大小就是不可以改变的。

- 其元素必须是相同类型，不允许出现混合类型。

- 数组中的元素可以是任何数据类型，包括基本类型和引用类型。

- 数组变量属于引用类型，数组也可以看成是对象，数组中的每个元素相当于该对象的成员变量。

  数组本身就是对象，JAVA中对象是在堆中的，因此数组无论保存原始类型还是其他对象类型，**数组对象本身是在堆中的**。

## 6.数组边界

- 下标的合法区间：[0,length-1],如果越界就会报错：

```java
    public static void main(String[] args) {    	int[] a  = new int[2];        System.out.printlen(a[2]);    }
```

- **ArrayIndexOutOfBoundsException:数组下标越界异常！**

- 小结

  - 数组是相同数据类型(数据类型可以为任意类型)的有序集合。

  - 数组也是对象。数组元素相当于对象的成变量。

  - 数组长度是确定的，不可变的。如果越界，则报

    ​	ArrayIndexOutOfBoundsException。 

## 7.数组的使用



- for-each循环

```java
package Array;public class demo03 {    public static void main(String[] args) {        int[] nums = {10,2,33,4,55,6};        //计算数组nums的元素之和        int Sum = 0;        for(int i = 0; i <nums.length;i++)        {            Sum += nums[i];        }           System.out.println("数组nums的和是"+Sum);        System.out.println("#####################");        //找出数组中的 最大值        int Max = 0;        for(int i = 0; i < nums.length;i++)        {            if(nums[i]> Max)            {                Max = nums[i];            }        }        System.out.println("数组nums的最大值是"+Max);    }}
```

 **增强型for-主要用于打印数组中的各个元素，如果要操作其中的元素就没那么适合了。**

```java
package Array;public class demo04 {    public static void main(String[] args) {        int[] nums = { 1,2,3,4,5,6};        for(int x : nums)        {            System.out.println(x);        }    }}
```

- 数组做方法入参&数组作为返回值

```java
package Array;public class demo04 {    public static void main(String[] args) {        int[] nums = { 1,2,3,4,5,6};        //打印数组元素        printfA(nums);        System.out.println();        //反转数组        int[] nums2 = reverse(nums);        printfA(nums2);    }    public static void printfA(int[] Array)    {        for(int i = 0 ; i< Array.length;i++)        {            System.out.print(Array[i]+" ");        }    }    //反转数组    public static int[] reverse(int[] arrays) {        int[] result = new int[arrays.length];        for(int i  = 0 , j = result.length-1; i <arrays.length;i++,j--)        {            result[j] = arrays[i];        }        return result;    }}
```

​	

## 8. 多维数组

- 多维数组可以看成是数组的数组，比如二维数组就是一个特殊的一维数组，其每个元素都是一个一维数组(在数组中存放数组)。
- 二维数组

```java
int[][] a = new	int[4][2];
```

- 解析：以上二维数组a，可以看成一个4行2列的数组。

  ```java
  package Array;public class demo05 {    public static void main(String[] args) {        //[4][2]        int[][] arrays = {{1,2},{2,3},{3,4},{4,5}};        for(int i = 0; i <arrays.length; i++)        {            for(int j = 0; j < arrays[i].length; j ++)            {                System.out.print(arrays[i][j]);            }            System.out.println("");        }    }}
  ```

  ## 9.Arrays类

  - 数组的工具类java.util.Arrays

  - 由于数组对象本身并没有什么方法可以供我们调用，但API中提供了一个工具类Arrays供我们使用，从而可以对数据对象进行一些基本的操作。

  - Arrays类中的方法都是static修饰的静态方法，在使用的时候可以直接使用类名进行调用，而"不用"使用对象来调用(注意是"不用"而不是"不能"。)

  - 具有一下常用功能：

    - 给数组赋值：通过fill方法。

    - 对数组排序：通过sor方法，按升序。

    - 比较数组：通过equals方法比较数组中元素是否相等。

    - 查找数组元素：通过binarySearch方法能对排序好的数组进行二分查找法操作。

      ##  9.冒泡排序

  - 冒泡排序无疑是最出名的排序算法之一，总共有八大排序。

  ```java
  package arrays;import java.util.Arrays;public class demo02 {    public static void main(String[] args) {        //冒泡排序：比较相邻的两个数，如果第 一个数比第二个数大，就交换他们俩个，        int[] nums = {1,23,323,35,2};        //输出原本的数组顺序        System.out.println(Arrays.toString(nums));        int[] sort = sort(nums); //调用完我们自己写的排序方法之后，返回一个排序后的数组        //打印输出nums数组        System.out.println(Arrays.toString(nums));    }    public static int[] sort(int[] arrays)    {        int temp = 0;        //如果数组已经排列好了，我们就没有必要再进行排列了        boolean flag = false;//通过flag标识位来减少没有意义的比较        //外层循环-判断我们要走多少次        for(int i = 0; i < arrays.length-1;i++)        {            //内层循环，比较两个数            for(int j = 0; j < arrays.length-1-i;j++)            {                if(arrays[j] > arrays[j+1])                {                    temp = arrays[j];                    arrays[j] = arrays[j+1];                    arrays[j+1] = temp;                    flag = true;                }                if(flag == false)                {                    break;                }            }        }        return arrays;    }}
  ```

  ## 9.稀疏数组

  - 需求:编写五子棋游戏中，有存盘退出和续上盘的功能。

  - 分析问题：因为该二维数组的很多值是默认值0，因此记录了很多没有意义的数据。

  - 解决:稀疏数组。

  - 什么是稀疏数组：

    - 当一个数组中大部分元素为0，或者为同一值的数组时，可以使用稀疏数组来保存该数组。

  - 稀疏数组的处理方式是：

    - 记录数组有几行几列，有多少个不同值。
    - 把具有不同值的元素和行列及值记录在一个小规模数组中，从而缩小程序的规模。
    - 例如：左边为原始数组，右边为稀疏数组。

    ![image-20210417192808182](/images/JAVA基础.assets/image-20210417192808182.png)

```java
package arrays;public class demo08 {    public static void main(String[] args) {            //创建一个 11*11的数组 0-没棋 1-黑棋 2-白棋        int[][] arrays1 = new int[11][11];        arrays1[1][2] = 1;        arrays1[2][3] = 2;        //输出原始的数组        System.out.println("原始的数组是");        for(int[] ints:arrays1)        {            for(int anInt:ints)            {                System.out.print(anInt+"\t");            }            System.out.println();        }        //转化为稀疏数组保存        //获取有效值的个数        int numbers = 0;        for(int i = 0 ; i<11; i++)        {            for(int j = 0; j < 11; j++)            {                if(arrays1[i][j] != 0)                numbers++;            }        }        System.out.println("数组中的有效值一共有"+numbers+"个");        //创建一个稀疏数组的数组        int[][] arrays2 = new int[numbers+1][3];        arrays2[0][0] = 11;        arrays2[0][1] = 11;        arrays2[0][2] = numbers;        //遍历二维数组，将非零的值存进稀疏数组        int count = 0 ;        for(int i = 0; i < arrays1.length;i++)        {            for(int j = 0; j <arrays1[i].length;j++) {                if (arrays1[i][j] != 0)                {                    count++;                    arrays2[count][0] = i;                    arrays2[count][1] = j;                    arrays2[count][2] = arrays1[i][j];                }            }        }        //输出稀疏数组        System.out.println("输出稀疏数组");        for(int i = 0; i < arrays2.length;i++)        {            System.out.println(arrays2[i][0]+"\t"+ arrays2[i][1]+"\t"+arrays2[i][2]+"\t");        }        System.out.println("========");        System.out.println("还原");        //读取稀疏数组        int[][] arrays3 = new int[arrays2[0][0]][arrays2[0][1]];        //给其中的元素还原它的值        for(int i = 1; i <arrays2.length;i++)        {            arrays3[arrays2[i][0]][arrays2[i][1]] = arrays2[i][2];        }        //打印        System.out.println("输出原始的数组");        for(int[] ints:arrays3)        {            for(int anInt:ints)            {                System.out.print(anInt+"\t");            }            System.out.println();        }    }}
```

# 5.面向对象编程

## 1.面向过程&面向对象

- 面向过程思想
  - 步骤清晰简单，第一步做什么，第二步做什么......(线性思维)
  - 面对过程适合处理一些较为简单的问题。
- 面向对象思想
  - 物以聚类，分类的思维模式，思考问题首先会解决问题需要哪些分类，然后对这些分类进行单独思考。最后，才对某个分类下的细节进行面向过程的思索。
  - 面向对象适合处理复杂的问题，适合处理需要多人协作的问题。
- **对于描述复杂的的事物，为了从宏观上把握，从整体上合理分析，我们需要使用面向对象的思路来分析整个系统。但是具体到微观操作 ，仍然需要面向过程的思路去处理。**

**面向对象编程的本质就是：以类的方式组织代码，以对象的组织(封装)数据。**

- 抽象——将事物的共性特点抽取出来。
- 三大特性：
  - 封装
  - 继承——父类、子类
  - 多态
- 从认识论角度考虑是先有对象后有类。对象，是具体的事物。类，是抽象的，是对对象的抽象。
- 从代码运行角度考虑是先有类后有对象。类是对象的模板。

## 2.方法的回顾及加深

- 方法的定义

  - 修饰符
  - 返回类型
  - **break和return的区别**
    - break跳出循环
    - return结束方法，返回一个结果，可以为空也可以为任意的其他类型
  - 方法名-注意规范
  - 参数列表-(参数类型、参数名)
  - 异常抛出

- 方法的调用

  - 静态方法-static
  - 非静态方法-
  - 形参和实参
  - 值传递和引用传递
  - this关键字

  ```java
  package FACE;public class demo02 {    public static void main(String[] args) {        //函数调用的两种方法        System.out.println(new demo02().Add(3,4));//非静态        System.out.println(Bdd(3,4));//静态    }    public static int Bdd(int a ,int b)    {        return a+b;    }    public int Add(int a ,int b)    {        return a+b;    }}
  ```

  ```java
  package FACE;//值传递 无法改变public class demo03 {    public static void main(String[] args) {        int a = 1;        System.out.println(a);        new demo03().change(a);        System.out.println(a);    }    public void change(int a )    {        a = 10;    }}
  ```

  ``` java
  package FACE;//引用传递：传递一个对象，本质还是值传递。public class demo04 {    public static void main(String[] args) {        Person person =  new Person();        System.out.println(person.name);        demo04.Change(person);        System.out.println(person.name);    }    public static void Change(Person person) {        //person是一个具体的类 指向一个人 Person person =  new Person();这是一个具体的人 可以改变属性        person.name = "傻逼";    }}class Person{    String name;//默认为Null}
  ```



## 3.类与对象的创建

(一个对象就相当于C语言里面的结构体)

- **类是一种抽象的数据类型，它是对某一类事物整体描述/定义，但是并不能代表某一个具体的事物。**
  - 动物、植物、手机、电脑......
  - Persoin类、Pet类、Car类等，这些类都是用来描述/定义某一类具体的事物应该具备的特点和行为。
- **对象是抽象概念的具体实例**
  - 张三就是一个人的具体实例，张三家里的旺财就是狗的一个具体实例。
  - 能够体现出特点，展现出功能的是具体的实例，而不是一个抽象的概念。

## 4.创建与初始化对象

- **使用new关键字创建对象。**
- 使用ne关键字创建的时候，除了分配内存空间之外，还会给创建好的对象进行默认的初始化及对类中构造器的调用。
- **构造器必须要掌握**

```java
//类是一个抽象的模板， 通过给它赋值来将它具体化package FACE;public class demo05 {   //属性：字段    String name ; //null     int age ; //0//方法    public void study()    {        System.out.println(this.name+"在学习");    }}
```

```java
//具体化package FACE;//一个项目应该只存一个main方法public class demo06 {    public static void main(String[] args) {        //类：抽象的，实例化。        //类实例化后会返回一个自己得对象        //demo05对象就是一个student类的具体实例！        demo05 xiaoming = new demo05();        demo05 xiaohong = new demo05();        xiaoming.name = "sb";        System.out.println(xiaoming.name);        System.out.println(xiaoming.age);    }}
```

## 5.构造器详解

- 类中的构造器也称为构造方法，是在进行创建对象的时候必须要调用的。并且构造器有以下两个特点：
  - 1.必须和类的名字相同
  - 2.必须没有返回类型，也不能写void

```java
package opp;public class Person {    //一个类即使什么都不写，也会存在一个方法    //显式定义的构造器    String name;    //实例化初始值    //使用new关键字，本质上实在调用构造器    public Person()    {    }    //有参构造:一旦定义了有参构造，无参定义就必须显示定义    public Person(String name)    {        this.name = name;    }}/**构造器：* 1必须和类名相同* 2没有返回值* 作用* 1new本质在调用构造方法* 2,初始化对象的值* 注意点：* 1.定义有参构造之后，如果想使用无参构造，显示的定义一个无参的构造* Alt+insert*/
```

```java
package opp;public class Application {    public static void main(String[] args) {        //使用new关键词实例化了一个对象        Person person = new Person("sb");        System.out.println(person.name);    }}
```

## 6.创建对象内存分析

![image-20210420110303023](/images/JAVA基础.assets/image-20210420110303023.png)

```java
package opp;import opp.Pet;public class App {    public static void main(String[] args) {        Pet dog = new Pet();        dog.name = "旺财";        dog.age =  3;        dog.shout();        System.out.println(dog.name);        System.out.println(dog.age);    }}
```

```java
package opp;public class Pet {    public String name;    public int age;    //无参构造    public void shout()    {        System.out.println("叫了一声");    }}
```

## 7.小结

```java
package opp;public class Summary {    public static void main(String[] args) {        /**         * 1.类与对象         *  类是一个模板：抽象，对象是一个具体的实例         * 2.方法         *  定义、调用！         * 3.对象的引用         *  引用类型：基本类型（8）         *  对象是通过引用来操作的：栈--》堆（地址）         * 4.属性：字段：Field 成员变量         *  默认初始化值：         *      数字： 0 0.0         *      char： u0000         *      bool:false（默认）         *      引用：         *          null         *   修饰符 属性类型 属性名 = 属性值！         * 5.对象的创建和使用         * -必须使用new关键字创造对象，构造器Person sb = new Person();         * -对象的属性 sb.name;         * -对象的方法 sb.sleep();         * 6.类         *  静态的属性         *  动态的行为         */        /    }}
```

## 8.封装

- 该露的露，该藏的藏
  - 我们设计程序的要求是"高内聚，低耦合"。高内聚就是类的内部数据细节自己完成 ，不允许外部干涉；低耦合:仅暴露少量的方法给外部使用。
- 封装(数据的隐藏)
  - 通常禁止访问一个对象中数据中的实际表示，而应通过操作接口来访问，这称为信息隐藏。
- **属性私有，get/set**

**alt+insert**

- 封装的意义：

  - 提高程序的安全性，保护数据
  - 隐藏代码的实现细节
  - 统一接口，系统的可维护性提高了

  ```java
  package opp;public class Student {    //属性私有    private String name;    private int id;    private char sex;    private int age;    //提供一些可以操作这个属性的方法    //提供一些public的get、set方法    //get获得这个数据    public String getName()    {        return this.name;    }    //set给这个数据设置值    public void setName(String name)    {        this.name =name;    }    public int getAge() {        return age;    }    public void setAge(int age) {        if(age>120 || age<0)        {            this.age = 3;        }        else{            this.age = age;        }    }}
  ```

  

  ```java
  package opp;import opp.Student;public class App2 {    public static void main(String[] args) {        Student s1 = new Student();        s1.setName("sb");        System.out.println(s1.getName());        s1.setAge(15);        System.out.println(s1.getAge());    }}
  ```

  ```java
  package opp;import opp.Student;public class App2 {    public static void main(String[] args) {        Student s1 = new Student();        s1.setName("sb");        System.out.println(s1.getName());        s1.setAge(15);        System.out.println(s1.getAge());    }}
  ```

  **判断两个方法是否相同看-方法名-参数**

## 9.继承

- 继承的本质是对某一批类的抽象，从而实现对现实世界更好的建模。
- extands的意思是"扩展"。子类是父类的扩展。
- JAVA中类只有单继承，没有多继承。(**一个儿子只有一个爸爸，一个爸爸有可以有多个儿子**)
- 继承是类和类之间的一种关系。除此之外，类和类之间的关系还有依赖、组合、聚合等。
- 继承关系的两个类，一个为子类(派生类)，一个为父类(基类)。子类继承父类，使用关键字extands来表示。
- 子类和父类之间，从意义上讲应该具有"is a"的关系。
- object类
- super(代表父,this代表当前)
- 方法重写

**子类继承了父类就会拥有父类的全部方法。**

**父类public的方法可以继承，private的方法无法继承。**

**Ctrl+h——显示继承的关系**

**在JAVA中所有的类都默认直接或间接继承object类**

***

**super注意点**

- super调用父类的构造方法，必须在构造方法的第一个。
- super必须只能出现在子类的方法或者构造方法中。
- super和this不能同时调用构造方法。

**和this对比**

- 代表的对象不同
  - this:本身调用者这个对象。
  - super:代表父类对象的应用。
- 前提：
  - this:没有继承也可以使用。
  - super:只能在继承条件下才可以使用。
- 构造方法：
  - this():本类的构造。
  - super():父类的构造。

***

**方法重写：**

**重写需要有继承关系，子类重写父类的方法**

- 方法名必须相同
- 参数列表必须相同
- 修饰符:范围可以扩大，但是不能缩小public->Portected>Default>private
- 抛出异常:可以被缩小，但不能扩大ClassNotFoundException-->Exception(大)

重写，子类的方法和父类必要一致，方法体不同。

**为什么要重写？**

- 父类的功能，子类不一定需要，或者不一定满足。

Alt+Insert:override

## 10.多态

- 即同一方法可以根据发送对象的不同而采用多种不同的行为方式。
- 一个对象的实际类型是确定的，但可以指向对象的引用的类型有很多。（父类、有关系的类）
- 多态存在的条件
  - 有继承关系
  - 子类重写父类方法
  - 父类引用指向子类对象
- 注意：多态是方法的多态，属性没有多态性。

```java
package DuoTai;    public class Student extends Person {        @java.lang.Override        public void run() {            System.out.println("son");        }        public void eat(){            System.out.println("eat");        }    }
```

```java
package DuoTai;import DuoTai.Student;import DuoTai.Person;public class Application {    public static void main(String[] args) {        //一个对象的实际类型是确定的        //new Student();        //new Person();        //可以指向的引用类型就不确定了        //父类的引用指向子类        //Student能调用的方法都是自己的或者继承父类的        Student s1 = new Student();        Person  s2 = new Student();        Object s3 = new Student();       //子类重写了父类的方法，执行子类的方法        s2.run();        s1.run();       //对象能执行那些方法只要看对象左边的类型，和右边关系不大        ((Student)s2).eat();//强制类型转换 高转低        s1.eat();    }}/*1.多态是方法的多态，属性没有多态2.父类和子类有联系 类型转换异常 ClassCastException3.多态存在的条件：有继承关系，方法需要重写（否则就是调用各自的方法，那就没有任何区别了）4. 父类的引用指向子类对象 Father f1 = new Son();Static是静态方法属于类，不属于实例final常量private方法：都没有重写 哪来的多态呢 */
```

****

**instanceof类型转换（与JAVA基础差不多，只是这里的类型转换指的是引用类型的转换）**

判断一个对象是什么类型 instanceof，判断两个类之间是否存在父子关系。

```java
package DuoTai;import DuoTai.Student;import DuoTai.Person;import DuoTai.Teacher;public class Application {    public static void main(String[] args) {        //System.out.println(x instanceof y);        //就是看x和y之间有没有父子，没有则报错        //Object - Person - Student        Object object = new Student();        System.out.println(object instanceof Student);        System.out.println("###########");        Student student = new Student();        System.out.println(student instanceof  Person);    }}
```

**static关键字详解**

```java
package Static;//static加在方法上叫静态方法，加在属性上叫静态属性public class Student {        private static  int age;//静态变量        private double score;//非静态变量        public  void run(){        }        public static void  go(){        }    public static void main(String[] args) {        Student s1 = new Student();        //静态可以这样调用        System.out.println(Student.age);        new Student().run();        //非静态        System.out.println(s1.age);        go();    }}
```

```java
package Static;public class Person {    //赋初始值    {        //匿名代码块（不建议这么写）        System.out.println("匿名代码块");    }    //只执行一次    static {        //静态代码块        System.out.println("静态代码块");    }    public Person(){        System.out.println("构造方法");    }    public static void main(String[] args) {        Person person1 =  new Person();        System.out.println("##############");        Person person2 =  new Person();    }}/** * 静态代码块 * 匿名代码块 * 构造方法 *///输出静态代码块匿名代码块构造方法##############匿名代码块构造方法
```

```java
package Static;//静态导入包import static java.lang.Math.random;public class demo01 {    public static void main(String[] args) {        System.out.println(random());//代替Math，random    }}//final表示该类不能被继承
```

## 11.抽象类

- abstract修饰符可以用来修饰方法也可以修饰类，如果修饰方法，那么该方法就是抽象方法，如果修饰类，那么该类就是抽象类。
- 抽象类中可以没有抽象方法，但是有抽象方法的类一定要声明为抽象类。
- 抽象类，不能使用new关键字类创建对象，它是用来让子类继承的。
- 抽象方法，只有方法的声明，没有方法的实现，它是用来让子类继承的。
- 子类继承抽象列，那么就必须要实现抽象类没有实现的抽象方法，否则该子类也要声明为抽象列

```java
package Static;//abstract抽象类//extends单继承 接口多继承(例如插座可以插很多个插头)public abstract class demo02 {    //约束，有人帮我们实现    //抽象方法 只有方法名字 没有方法实现    //抽象类的所有方法，继承了他的子类都要实现它的方法    //除非它的子类也是abstract,那就让它的子子类去实现......    public abstract void doSomething(){    }    /**     *不能new这个抽象类，只能靠子类去实现它，     *抽象类中可以写普通的方法     * 抽象方法必须在抽象类中     * 抽象的抽象 约束     * 存在的意义：抽象出来 提高开发效率 后期可扩展性比较高     * /    /}
```

## 12.接口

- 普通类:只有具体实现
- 抽象类：具体实现和规范(抽象方法)都有
- 接口:只有规范,自己无法写方法，约束和实现分离。
- 接口就是规范，定义的一组规则，体现了现实世界中的"如果你是... 则必须能..."的思想。如果你是天使，则必须能飞。如果你是汽车，则必须能跑。如果你是好人，则必须干掉坏人。......
- **接口的本质是契约**，就像我们人间的法律一样，制定了就必须要遵守。
- OO的精髓，是对对象的抽象，最能体现这一点的就是接口，为什么我们讨论设计模式都只针具备了抽象能力的语言(C++,java,c#等)，就是因为设计模式所研究的，实际上就是如何合理的去抽象。

**声明类的关键字是class,声明接口的关键字是interface**

```java
package dem0o01;//interface定义的关键字：接口都需要实现类//抽象的思维-架构师public interface UserService {    //接口中的所有定义其实都是抽象的public    //接口中定义常量    //一般来说是没有在接口中定义常量的    public static final int AGE = 99;    //返回类型+方法    //定义的方法都是public abstract    void  add(String name);    void  delete(String name);    void  update(String name);    void  query(String name);}
```

**作用:**

- 约束
- 定义一些方法，让不同的人来实现
- public abstract
- public static final
- 接口不能被实例化-因为接口中没有构造方法
- implements可以实现多个接口
- 必须重写接口中的方法

## 13.内部类

- 内部类就是在一个类的内部再定义一个类，比如，A类中定义一个B类，那么B类相对A类来说就成为内部类，而A类相对B类来说就是外部类了。
- 成员内部类
- 静态内部类
- 局部内部类
- 匿名内部类

```java
package dem0o01;public class Outer {    private int id = 10;    public void out(){        System.out.println("这是外部类的方法");    }    class Inner{        public void in(){            System.out.println("这是内部类的方法");        }        //获得外部类的私有属性        public void getID(){            System.out.println(id);        }    }}
```

```java
package dem0o01;public class Application {    public static void main(String[] args) {    //new-外部类        Outer outer = new Outer();        //通过这个外部类来实例化内部类        Outer.Inner inner = outer.new Inner();        inner.in();    }}
```

```java
package dem0o01;public class Outer {    //局部内部类    public void method(){        class Inner{        }    }}//一个java类中只能有一个public class//但能有多个class类class A{}
```

```java
package dem0o01;public class test {    public static void main(String[] args) {        // Apple apple = new Apple();        //没有名字初始化类-就是匿名初始化类        //不用将实例保存到变量中        new Apple().eat();    }}class Apple{    public void eat(){        System.out.println("1");    }}interface UserService{  }
```

(上面一些奇奇怪怪的创建类的方法不推荐使用，否则这个代码就只有你能看懂啦。)

# 6.异常

## 1.什么是异常

- 实际工作中，遇到的情况不可能是非常完美的。比如：你写的某个模块，用户输入不一定符合你的要求、你的程序要打开某个文件，这个文件可能不存在或者文件格式不对，你要读取数据库的数据，数据可能是空的等。我们的程序再跑着，内存或者硬盘可能满了。等等。
- 软件程序在运行过程中，非常可能遇到刚刚提到的这些异常问题，我们叫异常，英文是:Exception,意思是例外。这些，例外情况，或者叫异常，怎么让我们写的程序做出合理的处理。而不至于程序崩溃。
- 异常程序在运行过程中出现的不期而遇的各种状况，如：文件找不到、网络链接失败、非法参数等。
- 异常发生在程序运行期间，它影响了正常的程序执行流程。

## 2.简单分类

- 要理解JAVA异常处理是如何工作的，你需要掌握以下三种类型的异常：
- 检查性异常：最具代表的检查性异常是用户错误或问题引起的异常，这是程序员无法预见的。例如要打开一个不存在的文件时，一个异常就发生了，这些异常在编译时不能被简单地忽略
- 运行时异常：运行时异常是可能被程序员避免的异常。与检查性异常相反，运行时异常可以在编译时被忽略。
- 错误ERROR:错误不是异常，而是脱离程序员控制的问题。错误在代码中通常被忽略。例如，当栈溢出时，一个错误就发生了，它们在编译也检查不到的。

## 3.异常体系结构

- JAVA把异常当做对象来处理，并定义一个基类java.lang.Throwable作为所有异常的超累。
- 在JAVA API 中已经定义了许多异常类，这些异常类分为两大类，错误ERROR和异常Exception。

## 4.ERROR

- ERROR类对象由java虚拟机生成并抛出，大多数错误与代码编写者所执行的操作无关。
- JAVA虚拟机运行错误,当JVM不在有继续执行操作所需的内存资源时，将出现OutOfMemoryError。这些异常发生时，Java虚拟机(JVM)一般会选择线程终止；
- 还有发生在虚拟机试图执行应用时，如类定于(NoClassDefFoundError)、链接错误(LinkageError)。这些错误是不可查的，因为它们在应用程序的控制和处理能力之外，而却绝大多数是程序运行时不允许出现的状况。

## 5.Exception

![image-20210426201055911](/images/JAVA基础.assets/image-20210426201055911.png)

- 这些异常一般是由程序逻辑错误引起的，程序应该从逻辑角度尽可能避免这类异常的发生；
- Error和Exception的区别:Error通常是灾难性的致命的错误，是程序无法控制和处理的，当出现这些异常时，Java虚拟机(JVM)一般会选择终止线程；Exception通常情况下可以被程序处理的，并且在程序中应该尽可能的去处理这些异常。

## 6.异常处理机制

- 抛出异常
- 捕获异常
- 异常处理五个关键字
  - try、catch、finally、throw、throws 

（快捷键ctrl+alt+t快速选择语句将所选中的语句包裹起来）

```java
import sun.security.mscapi.CPublicKey;

import javax.naming.directory.AttributeInUseException;

public class Abnormal {
    public static void main(String[] args) {
        int a = 2;
        int b = 0;
        //监控异常
        try {//try监控区域
            System.out.println(a/b);
        }catch(ArithmeticException e){//捕获异常/里面的参数是捕获错误的类型
            System.out.println("程序出现异常啦");
        }finally{//处理善后工作
            System.out.printf("finally");
        }
    }
    public void test(int a,int b)
    {
        //假设在这个方法中处理不了异常，在方法上抛出异常。
        //主动抛出异常throw throws
        if(b == 0){
            throw new ArithmeticException();//主动抛出异常，一般在方法中使用
        }
    }
}
```



## 7.自定义异常

- 使用Java内置的异常类可以描述在编程时出现的大部分异常情况。除此之外，用户还可以自定义异常，只需要继承Exception类即可。
- 在程序中使用自定义异常类，大体可以分为一下几个步骤：

1. 创建自定义异常类。
2. 在方法中通过throw关键字抛出异常对象。
3. 如果在抛出异常的方法中处理异常，可以使用try-catch语句捕获并处理；否则在方法的声明处通过throws关键字指明要抛出给方法调用者的异常，继续进行下一步操作。
4. 在出现异常方法的调用者中捕获并处理异常。

```java
public class demo01 extends Exception {

    //传递数字>10;
    private int detail;

    public demo01(int a ){
        this.detail  = a;
    }

    //toString：异常的打印信息
    //实现了一个自定义的异常
    @Override
    public String toString() {
        return "demo01{" +
                "detail=" + detail +
                '}';
    }
}
```

```java
//测试类

public class Test {
    //可能会存在异常的方法
    static void test(int a) throws demo01{
        System.out.println("传递的参数为:"+a);
        if(a > 10)
        {
            throw new demo01(a);//抛出
        }
        System.out.println("OK");
    }
    public static void main(String[] args) {
        try {
            test(11);
        }catch (demo01 e)
        {
            System.out.println("MyException=>"+e);
        }
    }
}
```

![image-20210427165820171](/images/JAVA基础.assets/image-20210427165820171.png)

视频[【狂神说Java】Java零基础学习视频通俗易懂_哔哩哔哩 (゜-゜)つロ 干杯~-bilibili](https://www.bilibili.com/video/BV12J41137hu?t=17)

