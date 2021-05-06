***

相关视频——[C/C++技术教学：web 网络服务器开发！纯C语言手写web服务器，仅需 80 行代码，制作出你的专属服务器_哔哩哔哩 (゜-゜)つロ 干杯~-bilibili](https://www.bilibili.com/video/BV1uA411u7kD)

***

# 端口

1. 什么是端口？

   物理端口：电脑网口、USB、看的见的接口。

   虚拟端口：程序和网络进行通信的端口。

   端口就好比一个房子的门，是初入这个房子的必经之路。

2. 端口号

   端口是通过端口号来标记的，端口号只有整数，范围是从0到65535。

   （为什么最大是65535？）

3. 端口号怎么分配的

   端口号不是随意使用的，而是按照一定的规定进行分配。

4. 知名端口

   知名端口是众所周知的端口号，范围从0到1023，

    80端口分配给HTTP服务，

   21端口分配给FTP服务。

5. 动态端口

   动态端口的范围是从1024到65535，由操作系统进行分配。	

   之所以称为动态端口，是因为它一般不固定分配某种服务，而是动态分配。

   动态分配是指当一个系统进程或应用程序进程需要网络通信时，

   它向主机申请一个端口，主机从可用的端口号中分配一个供它使用。

   当这个进程关闭时，同时也就释放啦它所占用的端口号。

# Tcp服务器

如同接电话的过程一样，在程序中，如果想要完成一个tcp服务器的功能，需要的流程如下：

1. socket创建一个套接字
2. bind绑定ip和port
3. listen使套接字变为可以被动链接
4. accept等待客户端的链接
5. recv/send接收发送数据

# 代码实现

```c
//web Server
#include<stdio.h>
#include<stdbool.h>
#include<WinSock2.h>//包含网络编程的头文件，引入静态库
#pragma comment(lib,"ws2_32.lib")
bool isok;
int merror(int redata,int error,char* showinfo)
{	
	if (redata == error)
	{
		perror(showinfo);
		getchar();
		return -1;
	}
	return 0;
}
void sendhtml(SOCKET s, char* filename);
int main(void)
{	
	printf("weclome to my WebServer\n");
	WSADATA wsdata;
	WSAStartup(MAKEWORD(2,2),&wsdata);//确定socket版本信息
	//short两个字节2.2
	merror(isok,WSAEINVAL,"申请socket失败");
	//第一个参数-协议族，决定socket的地址类型
	//第二个参数-传输类型,SOCK_STREAM流传输
	//第三个参数-指定的传输协议，tcp
	SOCKET server = socket(AF_INET,SOCK_STREAM,IPPROTO_TCP);//使用af-inet,ipv4地址
	merror(server, INVALID_SOCKET, "创建socker失败");
	struct sockaddr_in seraddr;
	seraddr.sin_family = AF_INET;//和创建的时候一样，使用了Ipv4
	seraddr.sin_port = htons(80);//注意网络中的数据和电脑上的数据存储是有区别的，网络是大端存储，pc是小端存储
	seraddr.sin_addr.s_addr = INADDR_ANY;//监听任意的地址
	isok  = bind(server,&seraddr,sizeof(seraddr));
	merror(isok, SOCKET_ERROR, "绑定失败...\n");
	isok = listen(server, 5);
	merror(isok, SOCKET_ERROR, "监听失败...\n");
	struct sockaddr_in claddr;
	int cllen = sizeof(claddr);
	while (1)
	{
		SOCKET client = accept(server, &claddr, &cllen);//谁连进来了，发了多少数据
		merror(client, INVALID_SOCKET, "连接失败...\n");
		char revdata[1024] = "";
		recv(client,revdata,1024,0);
		printf("%s 共接收到%d字节数据\n", revdata,strlen(revdata));
		//如果下面这两行显示文字，测试发送成功。
		char sendata[1024] = "<h1 style =\" color:pink;\">hello,i'm sb</h1>";
		send(client,sendata,strlen(sendata),0);
		char* filename = "/";//填入文件名称xxx.html
		void sendhtml(client, filenama);
		closesocket(client);
	}
	closesocket(server);
	WSACleanup();
	getchar();
	return 0;
}
//打开文件-网页
//将文件放入项目文件夹下
void sendhtml(SOCKET s, char* filename)
{
	FILE* pfile = fopen(filename, "r");
	if (pfile == NULL)
	{
		printf("文件打开失败");
		return;
	}
	char temp[1024] = "";
	do
	{
		fgets(temp, 1024, pfile);
		send(s, temp, strlen(temp), 0);
	} while (!feof(pfile));
}
```



