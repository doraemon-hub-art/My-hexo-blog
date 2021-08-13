---
title: C++实现机房预约系统
date: 2021-08-14 01:19:19
tags:
  - -C++
  - -笔记
categories:
  - C++
cover: /img/c++.png 
---

***



相关视频——[黑马程序员C++](https://www.bilibili.com/video/BV1et411b73Z?p=282)(282-314)

(1-83笔记)——[链接](https://blog.csdn.net/qq_51604330/article/details/117753463?spm=1001.2014.3001.5501)

(84-146笔记)——[链接](https://blog.csdn.net/qq_51604330/article/details/118607922?spm=1001.2014.3001.5501)

(146-166笔记)——[链接](https://blog.csdn.net/qq_51604330/article/details/118652031?spm=1001.2014.3001.5501)

(167-263笔记)——[链接](https://blog.csdn.net/qq_51604330/article/details/119535438?spm=1001.2014.3001.5501)

(264-281笔记)——[链接](https://blog.csdn.net/qq_51604330/article/details/119601992?spm=1001.2014.3001.5501)

***



# C++实现机房预约系统

## 系统要求

![image-20210811183300379](/images/C++实现机房预约系统.assets/image-20210811183300379.png)

![image-20210811183310002](/images/C++实现机房预约系统.assets/image-20210811183310002.png)

![image-20210811183402942](/images/C++实现机房预约系统.assets/image-20210811183402942.png)

![image-20210811183501413](/images/C++实现机房预约系统.assets/image-20210811183501413.png)

![image-20210811183608553](/images/C++实现机房预约系统.assets/image-20210811183608553.png)

![image-20210811183620527](/images/C++实现机房预约系统.assets/image-20210811183620527.png)

![image-20210811183723618](/images/C++实现机房预约系统.assets/image-20210811183723618.png)

## 代码实现
**globalFile.h**
```c++
#pragma once

//管理员文件
#define ADMIN_FILE "admin.txt"
//学生文件
#define STUDENT_FILE "student.txt"
//教师文件
#define TEACHER_FILE "teacher.txt"
//机房信息文件
#define COMPUTER_FILE "computerRoom.txt"
//订单文件
#define ORDER_FILE "order.txt"
```
**computerRoom.h**
```c++
#pragma once
#include<iostream>
using namespace std;

class computerRoom
{
public:
	int m_ComId;//机房id号
	int m_ManNum;//最大容量
};
```
**Identity.h**
```c++
#pragma once
#include<iostream>
using namespace std;

//身份抽象基类
class Indentity
{
public:

	//操作菜单——纯虚函数
	virtual void operMenu() = 0;


	string m_UserName;
	string m_UserPassword;
};
```
**Student.h**
```c++
#pragma once
#include"Identity.h"
#include<iostream>
#include<string>
#include"computerRoom.h"
#include<vector>
#include<fstream>
#include"globalFile.h" 
#include"orderFile.h"
using namespace std;

//学生类
class Student :public Indentity
{
public:
	//默认构造
	Student();
	//有参构造
	Student(int id, string name, string pwd);
	//菜单界面
	virtual void operMenu();

	//申请预约
	void applyOrder();

	//查看自身预约
	void showMyOrder();
	
	//查看所有预约
	void showAllOrder();

	//取消预约
	void cancelOrder();
	
	//学号
	int m_Id;

	//机房容器
	vector<computerRoom>vCom;

};
```
**manager.h**
```c++
#pragma once
#include"Identity.h"
#include<iostream>
#include<string>
#include"globalFile.h"
#include<fstream>
#include<vector>
#include"Student.h"
#include"teacher.h"
#include<algorithm>
#include"computerRoom.h"
using namespace std;
class Manager :public Indentity
{
public:
	//默认构造
	Manager();
	
	//有参构造
	Manager(string name, string pwd);

	//菜单
	virtual void operMenu();

	//添加账号
	void addPerson();

	//查看账号
	void showPerson();

	//查看机房信息
	void showComputer();

	//清空预约记录
	void cleamFile();

	//初始化容器
	void initVector();

	//学生容器
	vector<Student>vStu;

	//教师容器
	vector<Teacher>vTea;

	//检测重复 学号、职工号  检测类型
	bool checkRepeat(int id, int type);

	//机房信息
	vector<computerRoom>vCom;
	
};

```
**teacher.h**
```c++
#pragma once
#include"Identity.h"
#include<iostream>
#include<string>
#include"orderFile.h"
#include<vector>
using namespace std;

class Teacher:public Indentity
{
public:
	//默认构造
	Teacher();

	//有参构造
	Teacher(int empId, string name,string pwd);


	//菜单
	virtual void operMenu();
	
	//查看所有预约
	void showAllOrder();

	//审核预约
	void validOrder();

	//职工号
	int m_EmpId;
};

```

**orderFile.cpp**
```c++
#include"orderFile.h"

//构造函数
OrderFile::OrderFile()
{
	ifstream ifs;
	ifs.open(ORDER_FILE, ios::in);

	string date;
	string interval;//时间段
	string stuId;
	string stuName;
	string roomId;
	string status;//状态

	this->m_Size = 0;//记录条数

	while (ifs >> date && 
		ifs >> interval && 
		ifs >> stuId && 
		ifs >> stuName && 
		ifs >> roomId && 
		ifs >> status)
	{
	/*	cout << date << endl;
		cout << interval << endl;
		cout << stuId << endl;
		cout << stuName << endl;
		cout << roomId << endl;
		cout<<status << endl;*/

		string key;
		string value;
		map<string, string>m;

		int pos = date.find(":");
		if (pos != -1)
		{
			key = date.substr(0, pos);
			value = date.substr(pos + 1, date.size() - pos - 1);
			m.insert(make_pair(key, value));
		}
		
		pos = interval.find(":");
		if (pos != -1)
		{
			key = interval.substr(0, pos);
			value = interval.substr(pos + 1, interval.size() - pos - 1);
			m.insert(make_pair(key, value));
		}

		pos = stuId.find(":");
		if (pos != -1)
		{
			key = stuId.substr(0, pos);
			value = stuId.substr(pos + 1, stuId.size() - pos - 1);
			m.insert(make_pair(key, value));
		}


		pos = stuName.find(":");
		if (pos != -1)
		{
			key = stuName.substr(0, pos);
			value = stuName.substr(pos + 1, stuName.size() - pos - 1);
			m.insert(make_pair(key, value));
		}


		pos = roomId.find(":");
		if (pos != -1)
		{
			key = roomId.substr(0, pos);
			value = roomId.substr(pos + 1, roomId.size() - pos - 1);
			m.insert(make_pair(key, value));
		}


		pos = status.find(":");
		if (pos != -1)
		{
			key = status.substr(0, pos);
			value = status.substr(pos + 1, status.size() - pos - 1);
			m.insert(make_pair(key, value));
		}
		
		//将小的map容器放到大的map容器中
		this->m_orderData.insert(make_pair(this->m_Size, m));
		this->m_Size++;
	}
	

	ifs.close();


	//测试大map
	/*for (map<int, map<string, string>>::iterator it = m_orderData.begin(); it != m_orderData.end(); it++)
	{
		cout << it->first << endl;
		for (map<string, string>::iterator mit = (*it).second.begin(); mit != (*it).second.end(); mit++)
		{
			cout << mit->first << " " << mit->second << " ";
		}
		cout << endl;
	}*/



	system("pause");
	system("cls");
}

//更新预约记录
void OrderFile::updateOrder()
{
	if (this->m_Size == 0)
	{
		return;
	}
	ofstream ofs(ORDER_FILE, ios::out | ios::trunc);
	for (int i = 0; i < this->m_Size; i++)
	{
		ofs << "date:" << this->m_orderData[i]["date"] << " ";
		ofs << "interval:" << this->m_orderData[i]["interval"] << " ";
		ofs << "stuId:" << this->m_orderData[i]["stuId"] << " ";
		ofs << "stuName:" << this->m_orderData[i]["stuName"] << " ";
		ofs << "roomId:" << this->m_orderData[i]["roomId"] << " ";
		ofs << "status:" << this->m_orderData[i]["status"] << endl;
	}
	ofs.close();
}


```

**manager.cpp**
```c++
#include"manager.h"

//默认构造
Manager::Manager()
{

}

//有参构造
Manager::Manager(string name, string pwd)
{
	this->m_UserName = name;
	this->m_UserPassword = pwd;

	//初识化容器 获取到所有文件中 学生、老师、信息
	this->initVector();

	//初始化机房信息
	ifstream ifs;
	ifs.open(COMPUTER_FILE, ios::in);

	computerRoom com;
	while (ifs >> com.m_ComId && ifs >> com.m_ManNum)
	{
		vCom.push_back(com);
	}
	ifs.close();



}

//菜单
void Manager::operMenu()
{
	cout << "欢迎管理员:" << this->m_UserName << "登录!"<<endl;
	cout << "——————————" << endl;
	cout << "——1.添加账号——" << endl;
	cout << "——2.查看账号——" << endl;
	cout << "——3.查看机房——" << endl;
	cout << "——4.清空预约——" << endl;
	cout << "——0.注销登录——" << endl;
	cout << "——————————" << endl;
	cout << "——————————" << endl;
	cout << "当前学生数量:" << vStu.size() << endl;
	cout << "当前老师数量:" << vTea.size() << endl;
	cout << "机房数量为:" << vCom.size() << endl;
	cout << "——————————" << endl;
	cout << "请选择您的操作:" << endl;
}

//添加账号
void Manager::addPerson()
{
	cout << "请输入添加账号的类型" << endl;
	cout << "1.添加学生" << endl;
	cout << "2.添加老师" << endl;

	string fileName;//操作文件名
	string tip;//提示id号
	ofstream ofs;//文件操作对象

	string errorTip;

	int select = 0;
	cin >> select;
	if (select == 1)
	{
		fileName = STUDENT_FILE;
		tip = "请输入学号";
		errorTip = "学号重复,重新输入!";
	}
	else
	{
		fileName = TEACHER_FILE;
		tip = "请输入职工编号";
		errorTip = "职工号重复,重新输入!";
	}

	ofs.open(fileName, ios::out | ios::app);
	int id;
	string name;
	string pwd;


	cout << tip << endl;

	while (1)
	{
		cin >> id;
		bool ret = checkRepeat(id, select);
		if (ret)//有重复
		{
			cout << errorTip << endl;
		}
		else
		{
			break;
		}
	}

	cout << "请输入姓名" << endl;
	cin >> name;

	cout << "请输入密码" << endl;
	cin >> pwd;

	//向文件添加数据

	ofs << id << " " << name <<" " << pwd << " " << endl;
	cout << "添加成功" << endl;

	//添加成功就立即将内容同步到当前容器中
	//解决添加一个人，人数翻倍bug,我认为是重复读取
	if (select == 1)
	{
		//添加学生——同步到当前容器中
		Student s;
		s.m_Id = id;
		s.m_UserName = name;
		s.m_UserPassword = pwd;
		vStu.push_back(s);
	}
	else if (select == 2)
	{
		//添加老师——同步到当前容器中
		Teacher t;
		t.m_EmpId = id;
		t.m_UserName = name;
		t.m_UserPassword = pwd;
		vTea.push_back(t);
	}

	system("pause");
	system("cls");

	ofs.close();

	//调用初始化容器接口
	//this->initVector();

	/*不知道是我漏掉了代码，还是代码本身的小漏洞，连续添加账号会出现显示bug和人员统计数量bug
	我认为是重复的vector存储，读取，本人有两种解决方法

	1.如上，去掉结尾的初始化容器，在上面的添加成功后，直接将当前的成员读进vector容器中
	2.理论可行，但是我没去实现，在原来的代码基础上，结尾调用初始化容器接口前，清理一下当前容器,vStu.clear();vTea.clear();

	这样解决了bug，并且保留了即时同步。:)

	*/

	
	
	


}

void printStudent(Student& s)
{
	cout << "学号:" << s.m_Id << " 姓名:" << s.m_UserName << " 密码:" << s.m_UserPassword << endl;
}

void printTeacher(Teacher& s)
{
	cout << "职工号:" << s.m_EmpId << " 姓名:" << s.m_UserName << " 密码:" << s.m_UserPassword << endl;
}


//查看账号
void Manager::showPerson()
{
	cout << "请选择查看内容" << endl;
	cout << "1.查看所有学生" << endl;
	cout << "2.查看所有老师" << endl;

	int select = 0;
	cin >> select;

	if (select == 1)
	{
		cout << "所有学生信息如下:" << endl;
		for_each(vStu.begin(), vStu.end(), printStudent);
	}
	else
	{
		cout << "所老师生信息如下:" << endl;
		for_each(vTea.begin(), vTea.end(), printTeacher);
	}

	system("pause");
	system("cls");
}

//查看机房信息
void Manager::showComputer()
{
	cout << "机房信息如下" << endl;
	for (vector<computerRoom>::iterator it = vCom.begin(); it != vCom.end(); it++ )
	{
		cout << "机房编号:" << it->m_ComId << " 机房最大容量:" << it->m_ManNum << endl;
	}

	system("pause");
	system("cls");
}

//清空预约记录
void Manager::cleamFile()
{
	ofstream ofs(ORDER_FILE, ios::trunc);
	ofs.close();

	cout << "清空成功" << endl;
	system("pause");
	system("cls");
}

//初始化容器
void Manager::initVector()
{
	//读取信息 学生 老师 
	ifstream ifs;
	ifs.open(STUDENT_FILE, ios::in);
	if (!ifs.is_open())
	{
		cout << "文件读取失败" << endl;
		return;
	}
	Student s;
	while (ifs >> s.m_Id && ifs >> s.m_UserName && ifs >> s.m_UserPassword)
	{
		vStu.push_back(s);
	}

	ifs.close();

	//读取信息 老师
	ifs.open(TEACHER_FILE,ios::in);
	Teacher t;
	while (ifs >> t.m_EmpId && ifs >> t.m_UserName && ifs>>t.m_UserPassword)
	{
		vTea.push_back(t);
	}
	ifs.close();
}

//检测重复 
bool Manager::checkRepeat(int id, int type)
{
	if (type == 1)
	{
		for (vector<Student>::iterator it = vStu.begin(); it != vStu.end(); it++)
		{
			if (id == it->m_Id)
			{
				return true;
			}
		}
	}
	else
	{
		for (vector<Teacher>::iterator it = vTea.begin(); it != vTea.end(); it++)
		{
			if (id == it->m_EmpId)
			{
				return true;
			}
		}
	}

	return false;
}
```

**teacher.cpp**
```c++
#include"teacher.h"


Teacher::Teacher()
{

}

//有参构造
Teacher::Teacher(int empId, string name,string pwd)
{
	//初始化属性
	this->m_EmpId = empId;
	this->m_UserName = name;
	this->m_UserPassword = pwd;

}


//菜单
void Teacher::operMenu()
{
	cout << "欢迎教师: " << this->m_UserName << "登录! " << endl;
	cout << "——————————" << endl;
	cout << "——1.查看所有预约——" << endl;
	cout << "——2.审核预约————" << endl;
	cout << "——0.注销登录———" << endl;
	cout << "——————————" << endl;
	cout << "请选择你的操作" << endl;
}

//查看所有预约
void Teacher::showAllOrder()
{
	OrderFile of;
	if (of.m_Size == 0)
	{
		cout << "无预约记录" << endl;
		system("pause");
		system("cls");
		return;
	}
	for (int i = 0; i < of.m_Size; i++)
	{
		cout << i + 1 << "、 ";
		cout << "预约日期: 周" << of.m_orderData[i]["date"];
		cout << " 时间段:  " << (of.m_orderData[i]["interval"] == "1" ? "上午" : "下午");
		cout << " 学号:  " << of.m_orderData[i]["stuId"];
		cout << " 姓名:  " << of.m_orderData[i]["stuName"];
		cout << " 机房编号:  " << of.m_orderData[i]["roomId"];
		string status = " 状态: ";

		if (of.m_orderData[i]["status"] == "1")
		{
			status += "审核中";
		}
		else if (of.m_orderData[i]["status"] == "2")
		{
			status += "预约成功";
		}
		else if (of.m_orderData[i]["status"] == "-1")
		{
			status += "预约失败，审核未通过";
		}
		else
		{
			status += "预约已取消";
		}
		cout << status << endl;
	}
	system("pause");
	system("cls");
}

//审核预约
void Teacher::validOrder()
{
	OrderFile of;
	if (of.m_Size == 0)
	{
		cout << "无预约记录" << endl;
		system("pause");
		system("cls");
		return ;
	}

	cout << "待审核的预约记录如下:" << endl;
	
	vector<int>v;

	int index = 0;
	for (int i = 0; i < of.m_Size; i++)
	{
		if (of.m_orderData[i]["status"] == "1")
		{
			v.push_back(i);
			cout << ++index << "、 ";
			cout << "预约日期: 周" << of.m_orderData[i]["date"];
			cout << " 时间段: " << (of.m_orderData[i]["interval"] == "1" ? "上午" : "下午");
			cout << "学生编号: " << of.m_orderData[i]["stuId"];
			cout << "学生姓名: " << of.m_orderData[i]["stuName"];
			cout << "机房编号: " << of.m_orderData[i]["roomId"];
			cout << "状态: 审核中 " << endl;
		}
	}
	cout << "请输入要审核的预约记录,0代表返回" << endl;
	int select = 0;//接收用户选择的预约记录
	int ret = 0;//接收预约结果记录
	while (1)
	{
		cin >> select;
		if (select >= 0 && select <= v.size())
		{
			if (select == 0)
			{
				break;
			}
			else
			{
				cout << "请输入审核结果" << endl;
				cout << "1.通过" << endl;
				cout << "2.不通过" << endl;
				cin >> ret;
				if (ret == 1)
				{
					//通过
					of.m_orderData[v[select - 1]]["status"] = "2";//-1是因为vector容器下标是从0开始的
				}
				else
				{
					//不通过
					of.m_orderData[v[select - 1]]["status"] = "-1";
				}
				//更新预约记录
				of.updateOrder();
				cout << "审核完毕" << endl;
				break;
			}
		}
		cout << "输入有误，请重新输入" << endl;
	}
	system("pause");
	system("cls");
}

```

**Student.cpp**
```c++
#include"Student.h"


Student::Student()
{

}
//有参构造
Student::Student(int id, string name, string pwd)
{
	//初始化属性
	this->m_Id = id;
	this->m_UserName = name;
	this->m_UserPassword = pwd;

	//初始化机房信息
	ifstream ifs;
	ifs.open(COMPUTER_FILE,ios::in);

	computerRoom com;
	while (ifs>>com.m_ComId && ifs >> com.m_ManNum)
	{
		//将读取的信息放入到容器中
		vCom.push_back(com);
	}

	ifs.close();
}
//菜单界面
void Student::operMenu()
{
	cout << "欢迎学生代表:" << this->m_UserName << "登录!"<<endl;
	cout << "——————————" << endl;
	cout << "——1.申请预约——" << endl;
	cout << "——2.查看我的预约—" << endl;
	cout << "——3.查看所有预约—" << endl;
	cout << "——4.取消预约——" << endl;
	cout << "——0.注销登录——" << endl;
	cout << "——————————" << endl;
	cout << "请选择您的操作:" << endl;

}

//申请预约
void Student::applyOrder()
{
	cout << "机房开放时间为周一至周五" << endl;
	cout << "请输入申请预约的时间" << endl;
	cout << "1.周一" << endl;
	cout << "2.周二" << endl;
	cout << "3.周三" << endl;
	cout << "4.周四" << endl;
	cout << "5.周五" << endl;

	//日期
	int data = 0;
	//时间段
	int interval = 0;
	//机房编号
	int room = 0;

	while (1)
	{
		cin >> data;
		if (data >= 1 && data <= 5)
		{
			break;
		}
		else
		{
			cout << "输入有误，请重新输入。" << endl;
		}
	}
	cout << "请输入申请预约的时间段" << endl;
	cout << "1.上午" << endl;
	cout << "2.下午" << endl;

	while (1)
	{
		cin >> interval;
		if (interval >= 1 && interval <= 2)
		{
			break;
		}
		else
		{
			cout << "输入有误，请重新输入。" << endl;
		}
	}

	cout << "请选择机房" << endl;
	for (int i = 0; i < vCom.size(); i++)
	{
		cout << vCom[i].m_ComId << "号机房容量为:" << vCom[i].m_ManNum<<endl;
	}

	while (1)
	{
		cin >> room;
		if (room >= 1 && room <= 3)
		{
			break;
		}
		else
		{
			cout << "输入有误，请重新输入。" << endl;
		}
	}
	cout << "预约成功，审核中！" << endl;

	//写入文件
	ofstream ofs;
	ofs.open(ORDER_FILE, ios::app);
	ofs << "date:" << data << " ";
	ofs << "interval:" << interval << " ";
	ofs << "stuId:" << this->m_Id << " ";
	ofs << "stuName:" << this->m_UserName << " ";
	ofs << "roomId:" << room << " ";
	ofs << "status:" << 1 << endl;

	ofs.close();

	system("pause");
	system("cls");
}

//查看自身预约
void Student::showMyOrder()
{
	OrderFile of;
	if (of.m_Size == 0)
	{
		cout << "无预约记录" << endl;
		system("pause");
		system("cls");
		return;
	}
	for (int i = 0; i < of.m_Size; i++)
	{
		//string 转 int
		//string 利用.c_str()转const char *
		//利用atoi (const char * )转 int
		if (this->m_Id == atoi(of.m_orderData[i]["stuId"].c_str()))//找到自身预约
		{
			cout << "预约日期:  周" << of.m_orderData[i]["date"];
			cout << " 时间段:  " << (of.m_orderData[i]["interval"] == "1" ? "上午" :"下午");
			cout << " 机房号:  " << of.m_orderData[i]["roomId"];
			string status = "状态:	";
			
			if (of.m_orderData[i]["status"] == "1")
			{
				status += "审核中";
			}
			else if (of.m_orderData[i]["status"] == "2")
			{
				status += "预约成功";
			}
			else if (of.m_orderData[i]["status"] == "-1")
			{
				status += "预约失败,审核未通过";
			}
			else
			{
				status += "预约已取消";
			}
			cout << status << endl;
		}
	}
	system("pause");
	system("cls");
}

//查看所有预约
void Student::showAllOrder()
{
	OrderFile of;
	if (of.m_Size == 0)
	{
		cout << "无预约记录" << endl;
		system("pause");
		system("cls");
		return;
	}

	for (int i = 0; i < of.m_Size; i++)
	{
		cout << i + 1 << "、 ";
		cout << "预约日期:  周" << of.m_orderData[i]["date"];
		cout << " 时间段:  " << (of.m_orderData[i]["interval"] == "1" ? "上午" : "下午");
		cout << " 学号:  " << of.m_orderData[i]["stuId"];
		cout << " 姓名:  " << of.m_orderData[i]["stuName"];
		cout << " 机房编号:  " << of.m_orderData[i]["roomId"];

		string status = " 状态: ";

		if (of.m_orderData[i]["status"] == "1")
		{
			status += "审核中";
		}
		else if(of.m_orderData[i]["status"] == "2")
		{
			status += "预约成功";
		}
		else if (of.m_orderData[i]["status"] == "-1")
		{
			status += "预约失败,审核未通过";
		}
		else
		{
			status += "预约已取消";
		}
		cout << status << endl;
	}
	system("pause");
	system("cls");
}

//取消预约
void Student::cancelOrder()
{
	OrderFile of;

	if (of.m_Size == 0)
	{
		cout << "无预约记录" << endl;
		system("pause");
		system("cls");
		return;
	}
	
	cout << "审核中或预约成功的记录可以取消，请输入取消的记录" << endl;
	
	//存放下标
	vector<int>v;

	int index = 1;
	for (int i = 0; i < of.m_Size; i++)
	{
		//找到自身预约的记录
		if (this->m_Id == atoi(of.m_orderData[i]["stuId"].c_str()))
		{
			//筛选状态
			//审核中或预约成功
			if (of.m_orderData[i]["status"] == "1" || of.m_orderData[i]["status"] == "2")
			{
				v.push_back(i);
				cout << index++ << "、 ";
				cout << "预约时期:  周" << of.m_orderData[i]["date"];
				cout << " 时间段:  " << (of.m_orderData[i]["interval"] == "1" ? "上午" : "下午");
				cout << " 机房编号:  " << of.m_orderData[i]["roomId"];
				string status = " 状态:  ";
				if (of.m_orderData[i]["status"] == "1")
				{
					status += "审核中";
				}
				else if(of.m_orderData[i]["status"] == "2")
				{
					status += "预约成功";
				}
				cout << status << endl;
			}
		}
	}
	cout << "请输入取消的记录，0代表返回" << endl;
	int select = 0;
	while (1)
	{
		cin >> select;
		if (select >= 0 && select <= v.size())
		{
			if (select == 0)
			{
				break;
			}
			else
			{
				of.m_orderData[v[select - 1]]["status"] = "0";

				//更新文件
				of.updateOrder();

				cout << "已取消预约" << endl;
				break;
			}
		}
		cout << "输入有误，请重新输入" << endl;
	}

	system("pause");
	system("cls");
}

```

**机房预约系统.cpp**

```c++
#include<iostream>
#include"Identity.h"
#include<fstream>
#include<string>
#include"globalFile.h"
#include"Student.h"
#include"teacher.h"
#include"manager.h"

using namespace std;

//进入教师子菜单界面
void teacherMenu(Indentity*& teacher)
{
	while (1)
	{
		//调用子菜单界面
		teacher->operMenu();

		Teacher* tea = (Teacher*)teacher;

		int select = 0;

		cin >> select;
		if (select == 1)//查看所有预约
		{
			tea->showAllOrder();
		}
		else if (select == 2)//审核预约
		{
			tea->validOrder();
		}
		else
		{
			delete teacher;
			cout << "注销成功" << endl;
			system("pause");
			system("cls");
			return;
		}
	}
}


//进入学生子菜单界面
void studentMenu(Indentity*& student)
{
	while (1)
	{
		//调用学生子菜单
		student->operMenu();

		Student* stu = (Student*)student;

		int select = 0;

		cin >> select;//接收用户选择
		
		if (select == 1)//申请预约
		{
			stu->applyOrder();
		}
		else if (select == 2)//查看自身预约
		{
			stu->showMyOrder();
		}
		else if (select == 3)//查看所有人预约
		{
			stu->showAllOrder();
		}
		else if(select == 4)//取消预约
		{
			stu->cancelOrder();
		}
		else//注销登录
		{
			delete student;
			cout << "注销成功!" << endl;

			system("pause");
			system("cls");
			return;
		}
	}
}


//进入管理员子菜单界面
void managerMenu(Indentity* &manager)
{
	while (1)
	{
		
		//调用管理员子菜单
		manager->operMenu();

		//将父类指针转为子类指针，调用子类里其他的接口
		Manager* man = (Manager*)manager;

		

		int select = 0;
		cin >> select;

		if (select == 1)//添加账号
		{
			//cout << "添加账号" << endl;
			man->addPerson();
		}
		else if (select == 2)//查看账号
		{
			//cout << "查看账号" << endl;
			man->showPerson();
		}
		else if (select == 3)//查看机房
		{
			//cout << "查看机房" << endl;
			man->showComputer();
		}
		else if (select == 4)//清空预约
		{
			//cout << "清空预约" << endl;
			man->cleamFile();
		}
		else
		{
			//注销
			delete manager;//销毁堆区对象
			cout << "注销成功" << endl;
			system("pause");
			system("cls");
			return ;
		}
	}
}

//登录功能
//文件名 身份类型
void LoginIn(string fileName, int type)
{
	Indentity * person = NULL;//父类指针指向子类对象
	
	//读文件
	ifstream ifs;
	ifs.open(fileName, ios::in);

	//判断文件是否存在
	if (!ifs.is_open())
	{
		cout << "文件不存在" << endl;
		ifs.close();
		return;
	}

	//准备接受用户的信息
	int id = 0;
	string name;
	string pwd;

	//判断身份
	if (type == 1)
	{
		cout << "请输入你的学号:" << endl;	
		cin >> id;
	}
	else if (type == 2)
	{
		cout << "请输入你的职工号:" << endl;
		cin >> id;
	}

	cout << "请输入用户名:" << endl;
	cin >> name;
	cout << "请输入密码:" << endl;
	cin >> pwd;


	if (type == 1)
	{
		//学生身份验证
		int fId;
		string fName;
		string fPwd;
		while (ifs >> fId && ifs >> fName && ifs >> fPwd)
		{
			//与用户输入的信息做对比、
			if (fId == id && fName == name && fPwd == pwd)
			{
				cout << "学生验证登录成功" << endl;
				system("pause");
				system("cls");

				person = new Student(id, name, pwd);
				//进入学生身份的子菜单
				studentMenu(person);
				return;
			}
		}
	}
	else if (type == 2)
	{
		//老师身份验证
		int fId;
		string fName;
		string fPwd;
		while (ifs >> fId && ifs >> fName && ifs >> fPwd)
		{
			//与用户输入的信息做对比、
			if (fId == id && fName == name && fPwd == pwd)
			{
				cout << "老师验证登录成功" << endl;
				system("pause");
				system("cls");

				person = new Teacher(id, name, pwd);
				//进入老师身份的子菜单
				teacherMenu(person);
				return;
			}
		}

	}
	else if (type == 3)
	{
		//管理员身份验证
	
		string fName;
		string fPwd;
		while (ifs >> fName && ifs >> fPwd)
		{
			//与用户输入的信息做对比、
			if (fName == name && fPwd == pwd)
			{
				cout << "管理员验证登录成功" << endl;
				system("pause");
				system("cls");

				person = new Manager(name, pwd);
				//进入管理员身份的子菜单
				managerMenu(person);

				return;
			}
		}
	}

	cout << "验证登录失败！" << endl;
	system("pause");
	system("cls");
	return;

}

int main(void)
{
	
	while (1)
	{

		cout << "——————————————" << endl;
		cout << "  欢迎使用机房预约系统系统" << endl;
		cout << "——————————————" << endl;
		cout << "\t1.学生代表" << endl;
		cout << "\t2.老  师" << endl;
		cout << "\t3.管理员" << endl;
		cout << "\t0.退  出" << endl;
		cout << "——————————————" << endl;
		cout << "请输入您的选择" << endl;



		int select = 0;


		cin >> select;
		switch (select)
		{
		case 1://学生
			LoginIn(STUDENT_FILE, 1);
			break;
		case 2://老师
			LoginIn(TEACHER_FILE, 2);
			break;
		case 3://管理员
			LoginIn(ADMIN_FILE, 3);
			break;
		case 0://退出
			cout << "欢迎下次使用!" << endl;
			system("pause");
			return 0;
			break;
		default:
			cout << "输入有误，请重新选择！" << endl;
			system("pause");
			system("cls");
			break;
		}
	}

	system("pause");
	return 0;
}
```
