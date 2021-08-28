---
title: 【网络编程】select模型
date: 2021-08-28 14:40:30
tags:
  - -网络编程
  - -C语言
  - -笔记
categories:
  - 网络编程
cover: /img/socket.png
---

# select模型

## 特点

1. 解决基本c/s模型中，accept，rcev傻等的问题。

   - 傻等阻塞
   - 执行阻塞 send recv accept 在执行的复制粘贴的过程中都是阻塞的。

   (网络模型就是解决阻塞问题的)

2. 实现多个客户端链接，与多个客户端分别通信。

3. 用于服务器，因为客户端就一个socket。

## 服务器端

```c
网络头文件 网络库
打开网络库
校验版本
创建socket
绑定地址与端口
开始监听
 
select    
```

### 逻辑

1. 每个客户端都有socket,服务器也有自己的socket,将所有的socket装进一个数据结构里，即数组。
2. 通过select函数，遍历1中的socket数组，当某个socket有相应，select就会通过其参数/返回值反馈出来。
3. 处理。如果见得到的是服务器socket，那就有客户端链接，调用accept。如果检测到客户端socket,那就是客户端请求通信，调用send或者recv。

### 定义一个装客户端的socket结构体

#### fd_set

是网络库中定义好的类型。

```c
typedef struct fd_set {
    	//几个有效的
        u_int fd_count;               /* how many are SET? */
        //数组
    	SOCKET  fd_array[FD_SETSIZE];   /* an array of SOCKETs */
} fd_set;

默认FD_SERSIZE 是64，重新宏定义要写在网络库前。
尽量不要太大，大用户量应该用更高级的网络模型。
select模型应用就是小用户量访问，几十几百，简单方便。
    
    fd_set socketClient;
```

#### 四个参数宏

```c
FD_ZERO
    #define FD_ZERO(set) (((fd_set FAR *)(set))->fd_count=0)
    将定义好的集合清零
    
    FD_ZERO(&socketClient);
```

```c
FD_SET 
    #define FD_SET(fd, set) do { \
    u_int __i; \
    for (__i = 0; __i < ((fd_set FAR *)(set))->fd_count; __i++) { \
        if (((fd_set FAR *)(set))->fd_array[__i] == (fd)) { \
            break; \
        } \
    } \
    if (__i == ((fd_set FAR *)(set))->fd_count) { \
        if (((fd_set FAR *)(set))->fd_count < FD_SETSIZE) { \
            ((fd_set FAR *)(set))->fd_array[__i] = (fd); \
            ((fd_set FAR *)(set))->fd_count++; \
        } \
    } \
} while(0, 0)
   向集合中添加socket  
```

```c
FD_CLR
   #define FD_CLR(fd, set) do { \
    u_int __i; \
    for (__i = 0; __i < ((fd_set FAR *)(set))->fd_count ; __i++) { \
        if (((fd_set FAR *)(set))->fd_array[__i] == fd) { \
            while (__i < ((fd_set FAR *)(set))->fd_count-1) { \
                ((fd_set FAR *)(set))->fd_array[__i] = \
                    ((fd_set FAR *)(set))->fd_array[__i+1]; \
                __i++; \
            } \
            ((fd_set FAR *)(set))->fd_count--; \
            break; \
        } \
    } \
} while(0, 0)
    从集合中删除某个元素，要手动释放，closesocket(socketServer)
    同链表删除。
```

```c
FD_ISSET
    #define FD_ISSET(fd, set) __WSAFDIsSet((SOCKET)(fd), (fd_set FAR *)(set))
    判断集合中是否有某个元素
    有-返回非0
    没有-返回0
```

### select

```c
int WSAAPI select(
	int nfds,
    fd_set *readfds,
    fd_set *writefds,
    fd_set *exceptfds,
);
```



#### 作用

监视socket集合，如果某个socket发生事件，(链接或者收发数据)，通过返回值以及参数告诉我们。

#### 参数1

Ignored忽略，填0，仅为了兼容(向下兼容性)Berkeley sockets。

#### 参数2

检查是否有**可读**的scoket。(是否有消息recv/accept/)

即客户端发来消息了，该socket就会被设置。

初始化所有的socket,通过select投放给系统，系统将有事件发生的socket再复制回来，调用后，这个参数就只剩下有请求的socket。

返回有响应的socket。用个中间变量接收。

#### 参数3

检查是否有**可写**的socket。

从头到尾遍历出来。

即，使可以给哪些客户端套接字发消息，即send,只要链接成功建立起来了，该客户端套接字就是可写的。

初始化所有的socket,通过select投放给系统，系统将可以写的socket在复制回来，调用后，这个参数就是装着可以被send数据的客户端socket。

#### 参数4

检查套接字上的异常错误，用法同参数23。将所有的socket投放进去。

得到异常套接字上的具体错误码。

getsockopt(socket,SOL_SOCKET,SO_ERROR,buf,buflen);

#### 参数5

最大等待时间，比如当客户端没有请求时，那么select函数可以等一会儿，一段时间过后，还没有，就继续执行select下面的语句，如果有了，就立刻执行下面的语句。

```c
TIMEVAL
    tv_sec 秒
    tv_usec 微秒
    0 0非阻塞状态，立刻返回
    3 4那就再无客户端相应的情况下等待3秒4微秒
NULL
    select完全阻塞，知道客户端有反应，我才继续
```

#### 返回值

```c
0 客户端在等待时间内没有反应  处理——continue>0 有客户端请求交流了SOCKET_ERROR 发生了错误    	得到错误码WSAGetLaseError()
```

### 流程总结

```c
socket集合
    socket判断有没有相应的
    	返回0，没有，继续挑
    	返回>0，有相应
    				可读的accept
    					  recv
    				可写的send
    				异常的getsockopt
    	SOCK_ERROR
```

select是阻塞的。

不等待——执行阻塞

半等待——执行阻塞+软阻塞

全等待——执行阻塞+硬阻塞 死等

### 完整代码

(仅熟悉流程)

```c
#define _WINSOCK_DEPRECATED_NO_WARNINGS 
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<WinSock2.h> 
#pragma comment(lib,"Ws2_32.lib")



//装所有的socket
fd_set allSocket;

BOOL WINAPI fun(DWORD dwCtrlType)
{
	switch (dwCtrlType)
	{

	case CTRL_CLOSE_EVENT:
		for (u_int i = 0; i < allSocket.fd_count; i++)
		{
			closesocket(allSocket.fd_array[i]);
		}
		WSACleanup();
	}
	return TRUE;
}

int main(void)
{
	//投递一个监视
	//关闭事件
	//控制台点叉退出
	SetConsoleCtrlHandler(fun, TRUE);	


	WORD wdVersion = MAKEWORD(2, 2);
	WSADATA wdSockMsg;
	int nRes = WSAStartup(wdVersion, &wdSockMsg);

	if (nRes != 0)
	{
		printf("网络库打开失败");
		return 0;
	}

	if (HIBYTE(wdSockMsg.wVersion) != 2 || LOBYTE(wdSockMsg.wVersion) != 2)
	{
		printf("版本不对");
		WSACleanup();
		return 0;
	}

	SOCKET socketServer = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);
	if (socketServer == INVALID_SOCKET)
	{
		printf("创建服务器socket失败");
		int a = WSAGetLastError();
		WSACleanup();
		return 0;
	}
	SOCKADDR_IN si;
	si.sin_family = AF_INET;
	si.sin_port = htons(12345);
	si.sin_addr.S_un.S_addr = inet_addr("127.0.0.1");

	if (SOCKET_ERROR == bind(socketServer, (const struct sockaddr*)&si, sizeof(si)))
	{
		printf("绑定错误");
		int a = WSAGetLastError();
		closesocket(socketServer);
		WSACleanup();
		return 0;
	}

	if (SOCKET_ERROR == listen(socketServer, SOMAXCONN))
	{
		int a = WSAGetLastError();
		closesocket(socketServer);
		WSACleanup();
		return 0;
	}


	//清零
	FD_ZERO(&allSocket);
	//把服务器装进去
	FD_SET(socketServer, &allSocket);
	
	while (1)
	{
		//可读
		fd_set readSocket = allSocket;
		//可写
		fd_set writeSocket = allSocket;
		FD_CLR(socketServer, &writeSocket);
		fd_set errorSocket = allSocket;



		//时间段
		struct timeval st;
		st.tv_sec = 3;
		st.tv_usec = 0;
		//不用哪个哪个位置就写NULL
		int nRes = select(0, &readSocket, &writeSocket, &errorSocket, &st);
		if (nRes == 0)//没有响应的socket
		{
			continue;
		}
		else if (nRes > 0)
		{

			for (u_int i = 0; i < errorSocket.fd_count; i++)
			{
				char str[100] = { 0 };
				int len = 99;
				if (SOCKET_ERROR == getsockopt(errorSocket.fd_array[i], SOL_SOCKET, SO_ERROR,str,&len))
				{
					printf("无法得到错误信息\n");
				}
				printf("%s\n", str);
				
			}

			for(u_int i = 0;i<writeSocket.fd_count;i++)
			{
				//printf("服务器%d %d:可写\n", socketServer, writeSocket.fd_array[i]);
				if (SOCKET_ERROR == send(writeSocket.fd_array[i], "ok", 2, 0))
				{
					//正常 大于0 socket_error 下线0
					int a = WSAGetLastError();
				}
			}

			//有响应
			//遍历socket
			for (u_int i = 0; i < readSocket.fd_count; i++)
			{
				if (readSocket.fd_array[i] == socketServer)
				{
					//有链接(响应)-accept
					SOCKET socketClient = accept(socketServer, NULL, NULL);
					if (socketClient == SOCKET_ERROR)
					{
						//链接出错
						continue;
					}
					FD_SET(socketClient, &allSocket);
					//SEND
					send(readSocket.fd_array[i], "服务器链接成功!", sizeof("服务器链接成功"),0);
				}
				else
				{
					char strBuf[1500] = { 0 };
					//客户端socket
					int nRecv = recv(readSocket.fd_array[i], strBuf, 1500, 0);
					if (nRecv == 0)
					{
						//客户端下线了
						//从集合中去掉		
						SOCKET socketTemp = readSocket.fd_array[i];
						FD_CLR(readSocket.fd_array[i],&allSocket);
						//释放
						closesocket(socketTemp);

					}
					else if(nRecv > 0)
					{
						//接收到了消息
						printf(strBuf);
					}
					else
					{
						//强制下线10054
						
						//出错了SOCK_ERROR
						int a = WSAGetLastError();
						switch (a)
						{
							case 10054:
							{
							SOCKET socketTemp = readSocket.fd_array[i];
							FD_CLR(readSocket.fd_array[i], &allSocket);
							closesocket(socketTemp);
							}
						}
						printf("%d", a);
					}
				}
			}
		}
		else
		{
			//发生错误了
			break;
		}
	}




	//释放socket集合
	for (u_int i = 0; i < allSocket.fd_count; i++)
	{
		closesocket(allSocket.fd_array[i]);
	}
	
	WSACleanup();//正常关闭
	system("pause");
	return 0;
}
```



