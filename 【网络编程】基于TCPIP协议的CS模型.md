---
title: 【网络编程】基于TCP/IP协议的C/S模型
date: 2021-08-26 15:45:06
tags: 
  - -网络编程
  - -C语言
  -	-笔记
categories:
  - 网络编程
cover: /img/socket.png
---

相关视频——[C3程序猿-windows网络编程：第一部分tcp/ip](https://www.bilibili.com/video/BV1cb411w7sZ)

# 基于TCP/IP协议的C/S模型

## TCP/IP协议

**全称**——Transmission Control Protocol / Internet Protocol

**重要性**——TCP/IP协议是今天互联网的基石，没有这个就上不了网

**概念**——TCP/IP协议族(簇，组，体系），并不是TCP协议和IP协议的总称，指的是整个网络传输体系。而TCP协议和IP协议就是单单的两个协议。

**特点**——**面向可连接的，可靠的，基于数据报的传输协议层**。

***

UDP/IP协议——面向非连接的，不可靠的，基于数据报的传输层协议。

***

## Client/Server客户端/服务器模型

C/S模型其实是概念层面的，实现层面可以是基于任何的网络协议。

常见的还有B/S模型——浏览器/服务器模型，基于http/https协议的

## 套接字编程与socket编程

socket中文——套接字

统称网络编程

## 使用

- 局域网
- 广域网——内网穿透，内网转发

## 服务端

### 网络头文件&网络库

是最底层的网络函数，QT、MFC、WPF等封装好的网络库都是对这些最本质的网络函数的二次封装。

**不区分大小写(windows)**

```c
#include<WinSock2.h>
//第二版的网络库，是一版的升级优化版本
#pragma comment(lib,"ws2_32.lib")
//.lib静态库后缀，是库文件，将.cpp文件编译为二进制文件
//好处:使用时无需编译，直接使用，解决时间
//32位编译环境和64位编译环境都用这个，没有ws2_64
```

###  打开网络库

**功能**：

打开网络库/启动网络库，启动了这个库，库里的函数才能使用，功能才能实现。

```c
int WSAStarp(
WORD wVersionRequired,
LPWSADATA lpWSAData
);  
```

#### 参数1

```c
参数1-使用哪个版本的网络库-WORD-无符号short
    WORD wdVersion = MAKEWORD(2, 1);
//主版本号2存在低数据位，副版本号1存在高数据位
```

***

（**参数前面有lp传地址**）

***



#### 参数2

```c
参数2-创建一个结构体，传递给系统，系统将信息放到结构体中，函数调用之后在外面通过结构体查看系统传递给我们的信息。
********************************************************************************
    WSADATA wdSockMsg;
********************************************************************************
其中包括
    struct WSAData {
        WORD                    wVersion;//我们要使用的版本
        WORD                    wHighVersion;//系统能提供给我们的最高的版本
        unsigned short          iMaxSockets;//返回可用的socket数量，2版本之后就没用了
        unsigned short          iMaxUdpDg;//UDP数据报信息的大小，2版本之后就没用了
        char FAR *              lpVendorInfo;//供应上特定的信息，2版本呢之后就没用 了
        
        char                    szDescription[WSADESCRIPTION_LEN+1];//当前库的描述信息，2.0是第二版的意思
        char                    szSystemStatus[WSASYS_STATUS_LEN+1];
        char                    szDescription[WSADESCRIPTION_LEN+1];
        char                    szSystemStatus[WSASYS_STATUS_LEN+1];
    }
********************************************************************************
    

********************************************************************************
	WSAStartup(wdVersion,&wdSockMsg);
********************************************************************************
```

```c
当输入的版本不存在
    例如:
    1.3 2.3——有主版本，没有副版本 得到主版本的的最大副版本 1.1 2.2并使用
    3.1 3.3——超过最大版 本号，使用系统能提供的最大版本2.2
    0.0 0.1 0.3——主版本是0，不支持请求的套接字版本
```



#### 返回值

**每一种错误有它唯一的对应码**

```c
if (nRes != 0)
	{
		printf("网络库打开失败");
		return 0;
	}

返回值-成功返回0
	 -失败返回对应错误的宏
    WSASYSNOTREADY   10091 
    	底层网络子系统尚未准备好进行网络通信。                     		 
    	系统配置问题，重启下电脑，检查ws2_32库是否存在，或者是否在环境配置目录下
	WSAVERNOTSUPPORTED 10092 
 		此特定Windows套接字实现不提供所请求的Windows套接字支持版本。      
   		要使用的版本不支持
	WSAEPROCLIM     10067  
    	已达到对Windows套接字实现支持的任务数量的限制。                                 	Windows Sockets实现可能限制同时使用它的应用程序的数量
	WSAEINPROGRESS 	10036          
   		正在阻止Windows Sockets 1.1操作。                                                 当前函数运行期间，由于某些原因造成阻塞，会返回在这个错误码，其他操作均禁止
	WSAEFAULT       10014          
    	lpWSAData参数不是有效指针。                                                                 参数写错了  
```



### 校验版本

```c
HIBYTE(wdSockMsg.wVersion) != 2 || LOBYTE(wdSockMsg.wVersion) != 2
    HIBYTE是高位-副版本
    LOBYTE是低位-主版本
   例如:只要有一个不是2，说明系统不支持我们要的2.2版本

       前面为主版本，后面为副版本
       要打开2.1
HIBYTE(wdSockMsg.wVersion) != 1 && LOBYTE(wdSockMsg.wVersion) != 2
       
       如果版本不对
       WSACleanup();//清理网络库
	   return 0;
```

### 创建socket

```c
SOCKET socket(
	int af,
    int type,
    int protocol
);
```

**例如**

```c
SOCKET socketServer = socket(AF_INET,SOCK_STREAM,IPPROTO_TCP);
```



#### **什么是socket**

将底层复杂的协议体系，执行流程，进行封装，封装完的结果，就是一个socket了。

也就是说，socket是我们调用协议进行通信的操作接口。

#### 意义

将复杂的协议过程与编程人员分开，我们只需要操作一个简单那的SOCKET就行了，对于底层的协议过程细节，我们完全不用知道，这就大大的方便了我们。

网络编程难在协议本身的复杂性，简单在我们编程层面完全不用考虑哪些。

#### 本质

就是一种数据类型。就是一个整数。

![image-20210823230157616](/images/基于TCP-IP协议的C-S模型.assets/image-20210823230157616.png)

socket的值是唯一的，通过这个值找到对应的协议。

#### 应用

网络通信的函数，全都要使用SOCKET,每个客户端有一个SOCKET，服务器有一个SOCKET，通信的时候，就需要这个SOCKET做参数，跟谁通信，就要传递谁的SOCKET。

SOCKET是网络封装的精华，写代码就是不停的使用SOCKET这个变量，所以又叫SOCKET编程。

#### 参数1

**地址的类型**

```c
加入你要与好友取得联系，可以通过
    电话、QQ、微信等方式
```

```c
AF_INET 2
    ipv4地址
    Internet协议版本地址系列
    例如:192.168.1.103
        0.0.0.0~255.255.255.255
        4字节，32位的地址
        点分十进制表示法
```

```c
AF_INET6 23
    ipv6地址
    Internet协议版本地址系列
    例如:2001:0:3238:DFE1:63::FEFB
        16字节，128位地址
```

```c
AF_BTH 32
    蓝牙地址
    例如:6B:2D:BC:A9:8C:12
```

```c
AF_IRDA 26
    红外数据协会(lrDA)地址
```

#### 参数2

**套接字类型**

```c
SOCK_STREAM 1
    提供给带有OOB数据传输机制的顺序，可靠，双向，基于连接的字节流。
    使用TCP作为internet地址系列AF_INET or AF_INET6
```

```c
SOCK_DGRAM 2
    固定(通常很小)最大长度的无连接，不可靠的缓冲区。
    使用UDP作为internet地址系列AF_INET or AF_INET6
```

```c
SOCK_RAW 3
    提供允许应用程序操作下一个上层协议头的原始套接字。 要操作IPv4标头，必须在套接字上设置IP_HDRINCL套接字选项。 要操作IPv6标头，必须在套接字上设置IPV6_HDRINCL套接字选项。
```

```c
SOCK_RDM 4
    提供可靠的消息数据报。 这种类型的一个示例是Windows中的实用通用多播（PGM）多播协议实现，通常称为可靠多播节目。仅在安装了可靠多播协议时才支持此类型值。
```

```c
SOCK_SEQPACKET 5
    提供基于数据报的伪流数据包。
```

#### 参数3

**协议类型**

```c
这个位置写0是什么意思？
    即系统给我们自动选择合适的协议。但不明确。
```

```c
IPPROTO_TCP
    传输控制协议（TCP）。 当af参数为AF_INET或AF_INET6且类型参数为SOCK_STREAM时，这是一个可能的值。
    可能的值是什么意思？
    如果有个协议TOP前两个参数也传这样的参数，此时(socket)第三个参数即写成IPPROTO_TOP
```

```c
IPPROTO_UDP 
    用户数据报协议（UDP）。 当af参数为AF_INET或AF_INET6且类型参数为SOCK_DGRAM时，这是一个可能的值。
```

```c
IPPROTO_ICMP
    Internet控制消息协议（ICMP）。 当af参数为AF_UNSPEC，AF_INET或AF_INET6且类型参数为SOCK_RAW或未指定时，这是一个可能的值。
```

```c
IPPROTO_IGMP
    Internet组管理协议（IGMP）。 当af参数为AF_UNSPEC，AF_INET或AF_INET6且类型参数为SOCK_RAW或未指定时，这是一个可能的值。
```

```c
IPPROTO_RM
    用于可靠多播的PGM协议。 当af参数为AF_INET且类型参数为SOCK_RDM时，这是一个可能的值。 在针对Windows Vista及更高版本发布的Windows SDK上，此协议也称为IPPROTO_PGM。
仅在安装了可靠多播协议时才支持此协议值。
```

#### 返回值

```c
成功-返回可用的socket
失败-不用了一定要释放掉——closesocket(xxx);
	然后再WSACleanup();清理网络库
   	注意二者的先后顺序，一定要先释放,然后再清理网路库，
    因为closesocket()是网络库中的函数。
********************************************************************************
失败——返回INVALID_SOCKET
   if (INVALID_SOCKET == socketServer)
	{
		int a = WSAGetLastError();//获取错误码
		WSACleanup();
		return 0;
	}
   //获取错误码——int a = WSAGetLastError();
   //检测在它上面离它最近的错误码    
```

### 绑定地址与端口

```c
int bind(
	SOCKET s,
	const sockaddr* addr,
	int namelen
);
```

#### 作用

给我们的socket绑定端口号和具体地址

**地址**：找到电脑，理论上只有一个。

**端口号**：找到电脑上对应软件的具体功能，每个通信的端口号是唯一的，同一个软件可能占用多个端口号。

#### 参数1

传递上面创建好的socket

(scoket绑定好地址类型、socket类型，协议类型)

(bind绑定实质的地址、端口号)

#### 参数2

![image-20210824222808154](/images/基于TCP-IP协议的C-S模型.assets/image-20210824222808154.png)

```c
struct sockaddr {
        ushort  sa_family;//地址类型
        char    sa_data[14];//端口号 ip地址
    //往一个字符串中赋值端口号和ip地址不好赋所以给出sockaddr_in,与之对应
};//16个字节

struct sockaddr_in {
        short   sin_family;//地址类型
        u_short sin_port;//端口号
        struct  in_addr sin_addr;//ip地址,4字节
        char    sin_zero[8];
};//16字节
//两个结构体大小和内存排布一样
```

```c
结构体
    -地址类型
     -ip地址	127.0.0.1-回送地址 本地回环地址 本地网络测试
    		  192.168.xxx.xxx- 用户ip地址
     -端口号  就是一个整数，0~65535。unsigned short
    	理论上0~65535都可以，但是0~1023为系统保留占用端口号
        	21端口分配给FTP(文件传输协议)服务
            25端口分配给SMTP（简单邮件传输协议）服务
            80端口分配给HTTP服务
    	所以真正的范围是1024~65535
      	端口是唯一的。
    打开cmd，输入netstat -ano 查看被使用的所有端口
    			netstat -aon|findstr "12345"检查我们要使用的端口是否被占用
    		
SOCKETADDR_IN为sockaddr提供方便
创建一个结构体SOCKETADDR_IN
为其中的结构体成员赋值
然后将它强转成sockaddr添加成功

    SOCKADDR_IN si;
	si.sin_family = AF_INET;//地址类型
	si.sin_port = htons(12345);//端口-将输入的unsigned short转换
	si.sin_addr.S_un.S_addr = inet_addr("127,0,0,1");//ip地址
	
	bind(socketServer, (const struct sockaddr*)&si, sizeof(si));
    
```

#### 返回值

```c
成功-返回0
失败-失败 返回宏SOCKET_ERROR
    		具体错误码通过WSAGetLastError()获得
    
if (SOCKET_ERROR == bind(socketServer, (const struct sockaddr*)&si, sizeof(si)))
{
	int a = GetLastError();	
	//释放
	closesocket(socketServer);
	//关闭网络库
	WSACleanup();
	return 0;
}
    
```

### 开始监听

```c
int WSAAPI listen(
	SOCKET s,
	int backing
);
```

```c
listen(socketServer, SOMAXCONN);
```



#### 作用

将套接字置于正在侦听传入连接的状态。

#### 参数1

服务器端的socket,也就是socket函数创建的。

#### 参数2

挂起连接的最大长度。(排队等待区)休息区的长度。

可以手动设置，可能是2~10，一般是**SOMAXCONN**让系统自己选择最合适的个数。不同系统的环境不一样，所以这个合适的数也都不一样。

#### WSAAPI

调用约定，是给操作系统看的，我们可以忽略它。

决定-函数名字的编译方式-参数的入栈顺序-函数的调用时间。

#### 返回值

```c
成功-返回0
失败-返回宏SOCKET_ERROR
     if (SOCKET_ERROR == listen(socketServer, SOMAXCONN))
	{
		//出错了
		int a = GetLastError();
		//释放
		closesocket(socketServer);
		//关闭网络库
		WSACleanup();
		return 0;
	}
```

### 创建客户端socket/接收连接

将每个客户端的信息都创建成一个socket。

```c
SOCKET WSAAPI accept(
	SOCKET s,
	sockaddr * addr,
	int *addrlen
);
```

#### 作用

accept函数允许在套接字上进行传入连接尝试。

listen监听客户端来的链接，accept将客户端的信息绑定到一个socket上，也就是给客户端创建一个socket,通过返回值得到客户端socket。

一次只能创建一个(返回值1个),有几个客户端链接，就要调用几次。

#### 参数1

(服务器socket)

#### 参数2

客户端的地址端口信息结构体，同bind的第二个参数

**意义**:系统帮我们监视着客户端的动态，肯定会记录客户端的信息，也就是IP地址，和端口号，并通过这个结构体记录。

只是这个我们不用自己填写结构体中的内容，**系统帮我们填写**。

```c
	//创建客户端
	struct sockaddr_in clientMsg;//客户端信息
	int len = 0;
	SOCKET socketClient = accept(socketServer, (struct sockaddr*)&clientMsg, &len);
```

#### 参数3

参数2的大小

****

**参数2、3也可以都写成NULL**，那就是不直接得到客户端的地址、端口号。

```c
可以通过
    getpeername(newSocket, (struct sockaddr*)&sockClient, &nLen);
得到客户端信息
	通过
    getsockname(sSocket, (sockaddr*)&addr, &nLen);
得到本地服务器信息
```

#### 返回值

```c
成功-返回客户端socket
失败-INVALID_SOCKET
    if (socketClient == INVALID_SOCKET)
	{
		closesocket(socketServer);
		closesocket(socketClient);
		WSACleanup();//清理网络库
		return 0;
	}
```

#### accept调试

1.阻塞，同步，当服务器没有客户端链接时，它会一直等待。

2.多个链接，一次只能连接一个，5个就要循环5次。同时客户端socket也要创建成数组,否则上一个的socket就丢了。

### 与客户端收发消息

消息从谁那来，要发送给谁，就写谁的socket

#### 收

得到指定客户端(参数1)发来的消息

```c
int recv(
	SOCKET s,
	char *buf,//消息，按字节
	int len,//长度
	int flags
);
```

```c
例:
	char buf[1500] = { 0 };
	int res = recv(socketClient, buf, 1499, 0);
```

### 完整代码

```c
#define _WINSOCK_DEPRECATED_NO_WARNINGS 
#define _CRT_SECURE_NO_WARNINGS
#include<WinSock2.h>
#include<stdio.h>
#include<string.h>
#pragma comment(lib,"ws2_32.lib")

int main(void)
{
	//打开网络库 
	WORD wdVersion = MAKEWORD(2, 2);
	WSADATA wdSockMsg;
	int nRes = WSAStartup(wdVersion,&wdSockMsg);

	if (nRes != 0)
	{
		printf("网络库打开失败");
		return 0;
	}
	//校验版本
	if (HIBYTE(wdSockMsg.wVersion) != 2 || LOBYTE(wdSockMsg.wVersion) != 2)
	{
		//说明版本不对
		printf("版本不对");
		WSACleanup();//清理网络库
		return 0;
	}
	//创建socket
	SOCKET socketServer = socket(AF_INET,SOCK_STREAM,IPPROTO_TCP);

	if (INVALID_SOCKET == socketServer)
	{
		printf("创建服务器socket失败");
		int a = WSAGetLastError();//获取错误码
		WSACleanup();
		return 0;
	}

	SOCKADDR_IN si;
	si.sin_family = AF_INET;//地址类型
	si.sin_port = htons(12345);//端口-将输入的unsigned short转换
	si.sin_addr.S_un.S_addr = inet_addr("127.0.0.1");//ip地址
	
	


	if (SOCKET_ERROR == bind(socketServer, (const struct sockaddr *)&si, sizeof(si)))
	{
		printf("绑定错误");
		int a = GetLastError();
		
		//释放
		closesocket(socketServer);
		//关闭网络库
		WSACleanup();
		return 0;
	}

	//开始监听
	if (SOCKET_ERROR == listen(socketServer, SOMAXCONN))
	{
		//出错了
		int a = GetLastError();
		//释放
		closesocket(socketServer);
		//关闭网络库
		WSACleanup();
		return 0;
	}


	//创建客户端
	struct sockaddr_in clientMsg;//客户端信息
	int len = sizeof(clientMsg);
	SOCKET socketClient = accept(socketServer, (struct sockaddr*)&clientMsg, &len);
	if (socketClient == INVALID_SOCKET)
	{
		printf("客户端链接失败");
		closesocket(socketServer);
		closesocket(socketClient);
		WSACleanup();//清理网络库
		return 0;
	}
	printf("客户端链接成功\n");


	//收发消息

	
	
	int sValue = send(socketClient, "服务器链接成功", sizeof("服务器链接成功"), 0);
	if (sValue == SOCKET_ERROR)
	{
		int a = WSAGetLastError();
		closesocket(socketClient);
		closesocket(socketServer);
		WSACleanup();
		return 0;
	 }

	while (1)
	{
		char buf[1500] = { 0 };
		int res = recv(socketClient, buf, 50, 0);
		if (res == 0)//客户端下线，链接中断
		{
			printf("链接中断、客户端下线\n");
		}
		else if (res == SOCKET_ERROR)
		{
			printf("出错了\n");
			int a = GetLastError();
			//根据实际情况处理
		}
		else
		{
			printf("客户端说:	%d  %s\n", res, buf);
		}

		scanf("%s", buf);
		send(socketClient, buf,strlen(buf), 0);
	}



	closesocket(socketServer);
	closesocket(socketClient);
	WSACleanup();//清理网络库

	system("pause");
	return 0;
}
```



##### 原理

本质就是复制。

数据的接收都是由协议本身做的，也就是socket的底层做的，系统会有一段缓冲区，存储着收到的数据。

recv的作用就是通过socket找到这个缓冲区，并把数据赋值进参数2。

##### 参数1

客户端的socket,每个客户端对应唯一的socket

##### 参数2

客户端消息的存储空间，是个字符数组，一般是1500字节。

网络传输的最大单元是1500字节，也就是客户端发过来的数据，一次最大就是1500字节，这是协议规定，很多情况总结出来的最优值。

##### 参数3

想要读取的字节个数。

一般是参数2的字节数-1，把/0字符串结尾留出来。

##### 参数4

数据的读取方式

一般就写个0。

```c
0
正常逻辑(自然性质)
    	从系统缓冲区里读，读走几个删几个，要不每次都从头开始读。
```

```c
MSG_PEEK
   		读完不删
    	不建议使用
```

```c
MSG_OOB
    	带外数据
    	就是传输一段数据，在外带一个额外的特殊数据。(小声bb)
    	不建议使用，读数据不行，无法计数。
```

```c
NSG_WAITALL
    	直到系统缓冲区字节数满足参数3所请求的字节数，才开始读取。
```

##### 返回值

```c
返回读出来字节大小，读没了就在recv函数卡着，等着客户端发来数据，即阻塞，同步。死等
客户端下线，返回0。
执行失败，返回SOCKET_ERROR    
    
    if (res == 0)//客户端下线，链接中断
	{
		printf("链接中断、客户端下线\n");
	}
	else if (res == SOCKET_ERROR)
	{
		printf("出错了\n");
		int a = GetLastError();
		//根据实际情况处理
	}
	else
	{
		printf("%d  %s\n", res, buf);
	} 
```



#### 发

```c
int WSAAPI send(
	SOCKET s,
	const char* buf,
	int len,
	int flags
);
```

##### 作用

向目标发送数据

**本质**：send函数将我们的数据复制粘贴进系统的协议发送缓冲区，计算机伺机发出去。

传输单元是1500字节。

##### 参数1

目标的socket，每个客户端对应唯一的socket

##### 参数2

给对方发送的字节串。1500

```c
这个大小不同的协议不一样，链路层14字节，ip头20字节，tcp头20字节，数据结尾还要有状态确认，加起来也几十个字节，数据结尾还要要状态确认，加起来也几十个字节，所以实际的数据位，不能写1500个，要留出来，例如1024，或者最多写1400，别多于1400是最好的。
```

```c
超过1500
    系统会分片处理，比如2000个字节
    系统分成两个包，1400+包头 == 1500 假设包头100字节
    				600+包头 == 700
    				分两次发送出去
    结果
    	系统要分包再打包，再发送，客户端收到之后还得拆包，组合数据，从而增加了系统的工作，降低了效率。
    	有的协议，就把分片后的二包直接丢了。
```

##### 参数3

字节个数。1400

##### 参数4

```c
0
MSG_OOB 同recv
MSG_DIBTROUTE指定数据不应受路由限制，Windows套接字服务提供程序可以选择忽略此标志。
```

##### 返回值

```c
成功-返回写入的字节数
失败-返回SOCKET_ERROR
    	WSAGetLastError()得到错误码
    	根据错误码信息做相应处理
    	-重启
    	-等待
    	-不用理会
```

## 客户端

```c
网络库头文件 网络库
    打开网络库
    校验版本
    创建SOCKET
```

### 链接到服务器

```c
int WSAAPI connect(
	SOCKET s,
	const sockaddr* name,
	int	namelen
);
```

```c
struct sockaddr_in serverMsg;
serverMsg.sin_family = AF_INET;
serverMsg.sin_port = htons(12345);//转换成网络字节序
serverMsg.sin_addr.S_un.S_addr = inet_addr("127.0.0.1");//服务器ip地址

connect(socketServer, (struct sockaddr*)&serverMsg, sizeof(serverMsg));
```

#### 作用

链接服务器并把服务器socket绑定到一起。

#### 参数1

服务器socket

#### 参数2

服务器ip地址端口号结构体

#### 参数3

参数2结构体大小

#### 返回值

```c
成功-返回0
失败-返回SOCKET_ERROR
    
```

***

**客户端与服务器收发消息，一发一接，一发一接，对应。**

***

### 完整代码

```c
#define _WINSOCK_DEPRECATED_NO_WARNINGS 
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<WinSock2.h>
#pragma  comment(lib,"Ws2_32.lib")
int main(void)
{
	WORD wdVersion = MAKEWORD(2, 2);
	WSADATA wdSockMsg;
	int nRes = WSAStartup(wdVersion, &wdSockMsg);
	if (nRes != 0)
	{
		printf("网络库打开失败");
		return 0;
	}
	//校验版本
	if (HIBYTE(wdSockMsg.wVersion) != 2 || LOBYTE(wdSockMsg.wVersion) != 2)
	{
		//说明版本不对
		printf("版本不对");
		WSACleanup();//清理网络库
		return 0;
	}
	//创建的是服务器的，客户端不用创建自己的socket，因为链接服务器，服务器会创建出来客户端的socket
	SOCKET socketServer = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);
	if (INVALID_SOCKET == socketServer)
	{
		int a = WSAGetLastError();
		WSACleanup();
		return 0;
	}

	//链接服务器
	struct sockaddr_in serverMsg;
	serverMsg.sin_family = AF_INET;
	serverMsg.sin_port = htons(12345);//转换成网络字节序
	serverMsg.sin_addr.S_un.S_addr = inet_addr("127.0.0.1");//服务器ip地址

	int cValue = connect(socketServer, (struct sockaddr*)&serverMsg, sizeof(serverMsg));
	if (cValue == SOCKET_ERROR)
	{
		int a = WSAGetLastError();
		closesocket(socketServer);
		WSACleanup();
		return 0;
	}

	//收发消息

	while (1)
	{
		char buf[1500] = { 0 };
		int res = recv(socketServer, buf, 50, 0);

		if (res == 0)//客户端下线，链接中断
		{
			printf("链接中断、客户端下线\n");
		}
		else if (res == SOCKET_ERROR)
		{
			printf("出错了\n");
			int a = GetLastError();
			//根据实际情况处理
		}
		else
		{
			printf("服务器说:	%d  %s\n", res, buf);
		}

		scanf("%s", buf);
		int sValue = send(socketServer, buf, strlen(buf), 0);
		if (sValue == SOCKET_ERROR)
		{
			int a = WSAGetLastError();
			closesocket(socketServer);
			WSACleanup();
			return 0;
		}
	}
	
	//清理网络库
	closesocket(socketServer);
	WSACleanup();
	system("pause");
	return 0;
}
```

## 延伸

由于accept，recv是阻塞的，做其中一件事，另一件事就做不了，等着接收客户端的消息-recv，这时来了个链接请求-accept无法处理。(没有目标的等待)

正确的处理方式是——哪个socket有请求就处理谁，得到连接请求，我们就直接accept,得到发来的消息，就recv。(有目的的等待，处理有请求的)

引出**select模型**。

