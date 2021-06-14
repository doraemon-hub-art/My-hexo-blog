---
title: LeetCode刷题(8)【栈&队列】用栈实现队列(C语言)
date: 2021-06-14 18:05:19
tags:
  - -数据结构
  - -LeetCode
  - -栈
  - -队列
  - -C语言
categories:
  - 数据结构
cover: /img/leetcode.png
---

**用栈实现队列**

[232. 用栈实现队列 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/implement-queue-using-stacks/)

***

相似题目——**用队列实现栈**

[LeetCode刷题(7)【栈&队列】用队列实现栈(C语言)_半生瓜のblog-CSDN博客](https://blog.csdn.net/qq_51604330/article/details/117826806?spm=1001.2014.3001.5501)



***

**思路**：

![image-20210614164351913](/images/LeetCode刷题8.assets/image-20210614164351913.png)



用栈实现队列要比用队列实现栈要简单一些，我们不用来回在两个栈里面导数据，只需要导一次，然后在依次出栈就成功实现队列的出队操作了。

![image-20210614164357870](/images/LeetCode刷题8.assets/image-20210614164357870.png)



![image-20210614165007351](/images/LeetCode刷题8.assets/image-20210614165007351.png)

**结论**：

1. 入数据往push栈里面入
2. 出数据从pop栈里面出，如果里面有数据，直接出，没有就把push栈里面的数据导过来，然后再出。

**代码实现**：

```c
typedef int StackDataType;
typedef struct Stack
{
	StackDataType* arry;
	int top;//指向栈顶
	int capacity;//栈的容量——能放几个数据
}Stack;
//初始化
void StackInit(Stack* ps)
{
	assert(ps);
	ps->arry = (StackDataType*)malloc(sizeof(StackDataType)*4);
	if (ps->arry == NULL)
	{
		printf("malloc fail");
		exit(-1);
	}
	ps->capacity = 4;
	ps->top = 0;
}
//销毁
void StackDestory(Stack* ps)
{
	assert(ps);
	free(ps->arry);
	ps->arry = NULL;
	ps->top = ps->capacity =0 ;
}
//入栈
void StackPush(Stack* ps, StackDataType x)
{
	assert(ps);
	//满了
	if (ps->top == ps->capacity)
	{
		StackDataType* tmp = (StackDataType*)realloc(ps->arry, ps->capacity * 2 * sizeof(StackDataType));
		if (tmp == NULL)
		{
			printf("realloc fail");
			exit(-1);
		}
		else
		{
			ps->arry = tmp;
			ps->capacity *= 2;
		}
	}
	ps->arry[ps->top] = x;
	ps->top++;
} 
//出栈
void StackPop(Stack* ps)
{
	assert(ps);
	//如果栈空了调用top，直接终止程序报错
	assert(ps->top > 0);
	ps->top--;
}
//返回栈顶元素
StackDataType StackTop(Stack* ps)
{
	assert(ps);
	assert(ps->top > 0);
	return ps->arry[ps->top - 1];
}
//返回栈中元素个数
int StackSize(Stack* ps)
{
	assert(ps);
	return ps->top;
}
//是否为空
bool StackEmpty(Stack* ps)
{
	assert(ps);
	return ps->top == 0;//真为空，假为非空。
}
//////////////////////////////////////////////////////////
typedef struct 
{
    Stack pushST;
    Stack popST;
} MyQueue;

/** Initialize your data structure here. */

MyQueue* myQueueCreate() 
{
    MyQueue* q = (MyQueue*)malloc(sizeof(MyQueue));
    if(q == NULL)
    {
        printf("malloc is fail!");
        return;
    }
    StackInit(&q->pushST);
    StackInit(&q->popST);
    return q;
}

/** Push element x to the back of queue. */
void myQueuePush(MyQueue* obj, int x) 
{
    //入数据就往pushST里面入
    StackPush(&obj->pushST,x);
}

/** Removes the element from in front of queue and returns that element. */
int myQueuePop(MyQueue* obj) 
{
    //类似于下面的peek
    // if(StackEmpty(&obj->popST))
    // {
    //     while(!StackEmpty(&obj->pushST))
    //     {
    //         StackPush(&obj->popST,StackTop(&obj->pushST));
    //         StackPop(&obj->pushST);
    //     }
    // }
    // int top = StackTop(&obj->popST);
    ///////////////////////////////////////////////////////////////
    //或者直接调用下面的myQueuePeek函数直接获取popST的栈顶元素
    //保存给top之后删除，然后return
    int top = myQueuePeek(obj);
    StackPop(&obj->popST);
    return top;
}

/** Get the front element. */
int myQueuePeek(MyQueue* obj) 
{
    //出数据要从popST里面出
    //如果popST里面是空的
    //就要先从pushST里面拿
    if(StackEmpty(&obj->popST))
    {
        //把pushST里面的元素导到popST里面
        //然后取第一个
        while(!StackEmpty(&obj->pushST))
        {
            //取pushST最上面的元素依次压进popST
            StackPush((&obj->popST),StackTop(&obj->pushST));
            StackPop(&obj->pushST);
        }
    }
    return StackTop(&obj->popST);
}

/** Returns whether the queue is empty. */
bool myQueueEmpty(MyQueue* obj) 
{
    return StackEmpty(&obj->popST) && StackEmpty(&obj->pushST);
}

void myQueueFree(MyQueue* obj) 
{
    StackDestory(&obj->pushST);
    StackDestory(&obj->popST);
    free(obj);
}

/**
 * Your MyQueue struct will be instantiated and called as such:
 * MyQueue* obj = myQueueCreate();
 * myQueuePush(obj, x);
 
 * int param_2 = myQueuePop(obj);
 
 * int param_3 = myQueuePeek(obj);
 
 * bool param_4 = myQueueEmpty(obj);
 
 * myQueueFree(obj);
*/
```

