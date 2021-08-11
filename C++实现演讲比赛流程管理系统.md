---
title: C++实现演讲比赛流程管理系统
date: 2021-08-11 12:01:46
tags:
  - -C++
  - -笔记
categories: 
  - C++
cover: /img/c++.png
---

***
相关视频——[黑马程序员C++](https://www.bilibili.com/video/BV1et411b73Z?p=147)(264-281)

(1-83笔记)——[链接](https://blog.csdn.net/qq_51604330/article/details/117753463?spm=1001.2014.3001.5501)

(84-146笔记)——[链接](https://blog.csdn.net/qq_51604330/article/details/118607922?spm=1001.2014.3001.5501)

(146-166笔记)——[链接](https://blog.csdn.net/qq_51604330/article/details/118652031?spm=1001.2014.3001.5501)

(167-263笔记)——[链接](https://blog.csdn.net/qq_51604330/article/details/119535438?spm=1001.2014.3001.5501)

***

# 演讲比赛流程管理系统

## 演讲比赛程序需求

![image-20210810111738845](C:/Users/xuanxuan/Desktop/C++实现演讲比赛流程管理系统.assets/image-20210810111738845.png)

### 程序功能

![image-20210810112344475](C:/Users/xuanxuan/Desktop/C++实现演讲比赛流程管理系统.assets/image-20210810112344475.png)

## 代码实现

**Speaker.h**

```c++
#pragma once
#include<iostream>
using namespace std;
class Speaker
{
public:
	string m_Name;
	double m_Score[2];//两轮得分
};
```

**SpeechManager.h**

```c++
#pragma once
#include<iostream>
#include<vector>
#include<map>
#include"Speaker.h"
#include<algorithm>
#include<deque>
#include<functional>
#include<numeric>
#include<string>
#include<fstream>
using namespace std;

//设计演讲管理类
class SpeechManager
{
public:
	SpeechManager();

	void Show_Menu();//菜单

	void ExitSystem();

	void InitSpeech();//初始化

	void CreatSpeaker();//创建选手

	void StartSpeech();//开始比赛

	void SpeechDraw();//抽签

	void SpeechContest();//比赛

	void ShowScore();//显示得分

	void SaveRecord();//保存文件

	void LoadRecord();//读取文件

	void ShowRecord();//显示往届数据

	bool FileIsEmpyt;//文件是否为空

	map<int, vector<string>>m_Record;//存放往届记录的容器
	~SpeechManager();
	
	void ClearRecord();//清空记录

	//第一轮比赛选手
	vector<int>v1;
	//第一轮晋级选手
	vector<int>v2;
	//胜出前三名选手编号容器
	vector<int>vVictory;
	//存放编号以及对应具体选手容器
	map<int, Speaker>m_Speaker;

	//存放比赛轮数
	int m_Index;


};


```

**SpeechManager.cpp**

```c++
#include"SpeechManager.h"


SpeechManager::SpeechManager()
{
	//初始化容器和属性
	this->InitSpeech();
	//创建选手12
	this->CreatSpeaker();
	//加载往届记录
	this->LoadRecord();

}


//菜单
void SpeechManager::Show_Menu()
{
	cout << "*********************" << endl;
	cout << "****欢迎参加演讲比赛" << endl;
	cout << "*****1.开始比赛*****" << endl;
	cout << "*****2.查看记录*****" << endl;
	cout << "*****3.清空记录*****" << endl;
	cout << "*****0.退出程序*****" << endl;
	cout << "*********************" << endl;
}

//退出
void SpeechManager::ExitSystem()
{
	cout << "欢迎下次使用" << endl;
	exit(0);
}
//初始化
void SpeechManager::InitSpeech()
{
	//容器置空
	this->v1.clear();
	this->v2.clear();
	this->vVictory.clear();
	this->m_Speaker.clear();
	//初始化比赛轮数
	this->m_Index = 1;


	//将记录的容器清空
	this->m_Record.clear();

}
//创建选手
void SpeechManager::CreatSpeaker()
{
	string NameSeed = "ABCDEFGHIJKL";
	for (unsigned int i = 0; i < NameSeed.size(); i++)
	{
		string name = "选手";
		name += NameSeed[i];
		//创建具体选手
		Speaker sp;
		sp.m_Name = name;
		for (int j = 0; j < 2; j++)
		{
			sp.m_Score[j] = 0;
		}
		//创建选手编号，并且放入到v1容器中
		this->v1.push_back(i + 10001);

		//选手编号以及对应选手放入map容器中
		this->m_Speaker.insert(make_pair(i + 10001, sp));
	}
}

//抽签
void SpeechManager::SpeechDraw()
{
	cout << "第" << this->m_Index << "轮比赛选手正在抽签" << endl;
	cout << "——————" << endl;
	if (this->m_Index == 1)
	{
		//第一轮
		random_shuffle(v1.begin(), v1.end());
		for (vector<int>::iterator it = v1.begin(); it != v1.end(); it++)
		{
			cout << *it << " ";
		}
		cout << endl;
	}
	else
	{
		//第二轮
		random_shuffle(v2.begin(), v2.end());
		for (vector<int>::iterator it = v2.begin(); it != v2.end(); it++)
		{
			cout << *it << " ";
		}
		cout << endl;
	}
}

//开始比赛
void SpeechManager::StartSpeech()
{
	//第一轮开始比赛

	//1.抽签
	this->SpeechDraw();
	//2.比赛
	this->SpeechContest();
	//3.显示晋级结果
	this->ShowScore();
	//第二轮比赛开始
	this->m_Index++;
	//抽签
	this->SpeechDraw();
	//比赛
	this->SpeechContest();
	//显示晋级结果
	this->ShowScore();
	//保存分数到文件中
	this->SaveRecord();

	//重置比赛
	//初始化属性
	this->InitSpeech();
	//创建选手
	this->CreatSpeaker();
	//获取往届记录
	this->LoadRecord();
	cout << "本届比赛已经结束！" << endl;

	system("pause");
	system("cls");



}

void SpeechManager::SpeechContest()
{
	cout << "第" << this->m_Index << "轮比赛正式开始" << endl;
	//准备临时容器存放小组成绩
	multimap<double, int, greater<double>>groupScore;

	int num = 0;//统计6个人为一组


	vector<int>v_Src;//比赛选手容器
	if (this->m_Index == 1)
	{
		v_Src = v1;
	}
	else
	{
		v_Src = v2;
	}

	//遍历所有选手进行比赛
	for (vector<int>::iterator it = v_Src.begin(); it != v_Src.end(); it++)
	{
		num++;
		//评委打分
		deque<double>d;
		for (int i = 0; i < 10; i++)
		{
			double score = (rand() % 401 + 600) / 10.f;
			//cout << score << " ";
			d.push_back(score);
		}
		//cout << endl;
		sort(d.begin(), d.end(), greater<double>());//排序
		//去除最高分和最低分
		d.pop_front();
		d.pop_back();

		double sum = accumulate(d.begin(), d.end(), 0.0f);//总分
		double avg = sum / (double)d.size();//平均分

		//将平均分放到map容器中
		this->m_Speaker[*it].m_Score[this->m_Index - 1] = avg;

		//打印平均分
		//cout << "编号" << " " << *it << "姓名" << " " << this->m_Speaker[*it].m_Name << " " << "平均分" << " " << avg << endl;

		//将打分数据 放入到临时小组中
		groupScore.insert(make_pair(avg, *it));//key是得分 value是具体选手编号
		//每6人取出前三名
		if (num % 6 == 0)
		{
			cout << "第" << num /6<< "小组比赛名次" << endl;
			for (multimap<double, int, greater<double>>::iterator it = groupScore.begin(); it != groupScore.end(); it++)
			{
				cout << "编号: " << it->second << "姓名: " << this->m_Speaker[it->second].m_Name << "成绩: " << this->m_Speaker[it->second].m_Score[this->m_Index-1] << endl;
			}
			//取走前三名
			int count = 0;
			for (multimap<double, int, greater<double>>::iterator it = groupScore.begin(); it != groupScore.end() && count < 3; it++, count++)
			{
				if (this->m_Index == 1)
				{
					v2.push_back((*it).second);
				}
				else
				{
					vVictory.push_back((*it).second);
				}
			}
			groupScore.clear();//小组容器清空
		}
	}
	cout << "第" << this->m_Index << "轮比赛完毕" << endl;
	system("pause");
}


//显示得分
void SpeechManager::ShowScore()
{
	cout << "第" << this->m_Index << "轮晋级选手如下" << endl;
	vector<int>v;
	if (this->m_Index == 1)
	{
		v = v2;
	}
	else
	{
		v = vVictory;
	}

	for (vector<int>::iterator it = v.begin(); it != v.end();it++)
	{
		cout << "选手编号: " << *it << "姓名: " << this->m_Speaker[*it].m_Name << "得分: " << this->m_Speaker[*it].m_Score[this->m_Index - 1] << endl;
	}
	system("pause");
	system("cls");
	this->Show_Menu();
}

//保存文件
void SpeechManager::SaveRecord()
{
	ofstream ofs;
	ofs.open("speech.csv", ios::out | ios::app);
	//将每个选手的数据写入到文件中
	for (vector<int>::iterator it = vVictory.begin(); it != vVictory.end(); it++)
	{
		ofs << *it << "," << this->m_Speaker[*it].m_Score[1] << ",";
	}
	ofs << endl;

	ofs.close();
	cout << "记录已经完成" << endl;

	//更改文件不为空状态
	this->FileIsEmpyt = false;
}

//读取文件
void SpeechManager::LoadRecord()
{
	ifstream ifs("speech.csv", ios::in);
	if (!ifs.is_open())
	{
		this->FileIsEmpyt = true;
		
		ifs.close();
		return;
	}

	//文件清空情况
	char ch;
	ifs >> ch;
	if (ifs.eof())
	{
		cout << "文件为空" << endl;
		this->FileIsEmpyt = true;
		ifs.close();
		return;
	}

	//文件不为空情况
	this->FileIsEmpyt = false;
	ifs.putback(ch);
	string data;
	int index = 0;
	while (ifs >> data)
	{
		vector<string>v;//存放冠军数据
		//cout << data << endl;
		int pos = -1;
		int start  = 0;
		while (1)
		{
			pos = data.find(",", start);
			if (pos == -1)
			{
				//没找到
				break;
			}
			string temp = data.substr(start, pos - start);
			//cout << temp << endl;
			v.push_back(temp);
			start = pos + 1;
		}
		this->m_Record.insert(make_pair(index, v));
		index++;
	}
	ifs.close();

	/*for (map<int, vector<string>>::iterator it = m_Record.begin(); it != m_Record.end(); it++)
	{
		cout <<"第"<<it->first << "届冠军编号：" << it->second[0] << "分数：" << it->second[1] << endl;
	}*/
}
//显示往届记录
void SpeechManager::ShowRecord()
{
	if (this->FileIsEmpyt)
	{
		cout << "文件为或者文件不存在" << endl;
	}
	for (int i = 0; i < this->m_Record.size(); i++)
	{
		cout << "第" << i + 1 << "届 "
			<< "冠军编号: " << this->m_Record[i][0] << "得分: " << this->m_Record[i][1] << " "
			<< "亚军编号: " << this->m_Record[i][2] << "得分: " << this->m_Record[i][3] << " "
			<< "季军编号: " << this->m_Record[i][4] << "得分: " << this->m_Record[i][5] << endl;
	}

	system("pause");
	system("cls");
}

//清空记录
void SpeechManager::ClearRecord()
{
	cout << "是否确定清空文件?" << endl;
	cout << "1.是、2.否" << endl;

	int select = 0;
	cin >> select;
	if (select == 1)
	{
		ofstream ofs("speech.csv",ios::trunc);
		ofs.close();

		//初始化容器和属性
		this->InitSpeech();
		//创建选手12
		this->CreatSpeaker();
		//加载往届记录
		this->LoadRecord();

		cout << "清空成功!" << endl;
	}
	system("pause");
	system("cls");
}

SpeechManager::~SpeechManager()
{

}
```

**演讲比赛流程管理系统.cpp**

```c++
#include<iostream>
#include<string>
#include<ctime>
#include"SpeechManager.h"
using namespace std;
int main(void)
{

	//随机数种子
	srand((unsigned int)time(NULL));

	//创建管理类
	SpeechManager sm;

	/*for (map<int, Speaker>::iterator it = sm.m_Speaker.begin(); it != sm.m_Speaker.end(); it++)
	{
		cout << it->first << " " << it->second.m_Name<<" "<<it->second.m_Score[0]<<endl;
	}*/

	int choice = 0;//用于存储用户输入
	
	while (1)
	{
		sm.Show_Menu();
		cin >> choice;
		switch (choice)
		{
		case 1://开始比赛
			sm.StartSpeech();
			break;
		case 2://查看往届比赛
			sm.ShowRecord();
			break;
		case 3://清空
			sm.ClearRecord();
			break;
		case 0://退出
			sm.ExitSystem();
			break;


		default:
			system("cls");
			break;
		}
	}

	system("pause");
	return 0;
}
```

