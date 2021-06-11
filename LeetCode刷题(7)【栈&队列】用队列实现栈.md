---
title: LeetCode刷题(7)【栈&队列】用队列实现栈
date: 2021-06-11 19:41:18
tags:
  - -C语言
  - -LeetCode
  - -数据结构
  - -栈
  - -队列
categories: 
 - 数据结构
cover: /img/leetcode.png
---

**用队列实现栈**

[225. 用队列实现栈 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/implement-stack-using-queues/)

![image-20210611195234710](/images/LeetCode7.assets/image-20210611195234710.png)

**目的**：用队列实现栈，从先进先出——>先进后出，

1234这四个数据依次从队列1的队尾进入，要让4先出，一个队列是无法实现的，所以这里的队列2就排上用场了，我们可以利用队列2来进行导数据。

![image-20210611195716778](/images/LeetCode7.assets/image-20210611195716778.png)

将123依次由队列2的队尾进入到队列2中，此时队列1中还剩一个4，将4弹出，同理，再将12依次进入到队列1中，将3弹出......

**也就是说**。

出数据把不为空的 队列数据向为空的队列中导，知道剩最后一个。

入数据向不为空的队列入。

始终保持一个队列为空，一个不为空。

***

**队列的实现**——队列的实现——[【线性表】之队列_半生瓜のblog-CSDN博客](https://blog.csdn.net/qq_51604330/article/details/117697458)

***

```c
typedef int QueueDataType;
typedef struct QueueNode
{
	struct QueueNode* next;
	QueueDataType data;
}QueueNode;
//单链表除了尾插还要尾删，所以不会加这个
typedef struct Queue
{
	QueueNode* tail;
	QueueNode* head;
}Queue;
//初始化
void QueueInit(Queue* pq)
{
	assert(pq);
	pq->tail = pq->head = NULL;
}
//销毁
void QueueDestory(Queue* pq)
{
	assert(pq);
	QueueNode* cur = pq->head;
	while (cur)
	{
		QueueNode* curNext = cur->next;
		free(cur);
		cur = curNext;
	}
	pq->head = pq->tail = NULL;
}
//队尾入
void QueuePush(Queue* pq, QueueDataType x)
{
	assert(pq);
	QueueNode* newnode = (QueueNode*)malloc(sizeof(QueueNode));
	if (newnode == NULL)
	{
		printf("malloc is fail\n");
		exit(-1);
	}
	newnode->data = x;
	newnode->next = NULL;

	if (pq->tail == NULL)
	{
		pq->head = pq->tail = newnode;
	}
	else
	{
		pq->tail->next = newnode;
		pq->tail = newnode;
	}
}
//队头出
void QueuePop(Queue* pq)
{
	assert(pq);
	assert(pq->head);//队列是不等于空的
	if (pq->head->next == NULL)
	{
		free(pq->head);
		pq->head = pq->tail = NULL;
	}
	else
	{
		//先保存一下下一个结点
		QueueNode* nextNode = pq->head->next;
		free(pq->head);
		pq->head = nextNode;
	}
}
//队头数据
QueueDataType QueueFront(Queue* pq)
{
	assert(pq);
	assert(pq->head);
	return pq->head->data;
}
//队尾数据
QueueDataType QueueBack(Queue* pq)
{
	assert(pq);
	assert(pq->head);
	return pq->tail->data;
}
//是否为空
bool QueueEmpty(Queue* pq)
{
	assert(pq);
	return pq->head == NULL;
}
//返回数据个数
int QueueSize(Queue* pq)
{
	assert(pq);
	int size = 0;
	QueueNode* cur = pq->head;
	while (cur)
	{
		size++;
		cur = cur->next;
	}
	return size;
}
typedef struct {
    //创建两个队列
    Queue q1;
    Queue q2;
} MyStack;
//creat a queue above this
/** Initialize your data structure here. */

MyStack* myStackCreate() 
{
 //保证出了函数还在
    MyStack* ps = (MyStack*)malloc(sizeof(MyStack));
    if(ps == NULL)
    {
        printf("malloc is fail!\n");
        exit(-1);
    }
    QueueInit(&ps->q1);
    QueueInit(&ps->q2);
    return ps;
}

/** Push element x onto stack. */
void myStackPush(MyStack* obj, int x) 
{
    //谁为空就往谁里面入
    if(!QueueEmpty(&obj->q1))
    {
        QueuePush(&obj->q1,x);
    }
    else
    {
        QueuePush(&obj->q2,x);
    }
}

/** Removes the element on top of the stack and returns that element. */
int myStackPop(MyStack* obj) 
{
    //谁不为空就把谁中的元素导到另一个队列中，直到该队列只剩一个元素
    //先假设一个队列不为空一个队列为空，如果不是这样，就交换一下
    Queue* emptyQ = &obj->q1;
    Queue* noemptyQ = &obj->q2;
    if(!QueueEmpty(&obj->q1))
    {
        emptyQ = &obj->q2;
        noemptyQ = &obj->q1;
    }
    //然后循环，进行导元素，将noemptyQ的导入emptyQ中
    while(QueueSize(noemptyQ)>1)
    {
        QueuePush(emptyQ,QueueFront(noemptyQ));
        //出一个删一个
        QueuePop(noemptyQ);
    }
    //接口要求——返回栈顶的元素，就是noemptyQ中剩下的那个元素
    int top = QueueFront(noemptyQ);
    QueuePop(noemptyQ);
    return top;
}

/** Get the top element. */
int myStackTop(MyStack* obj) 
{
    //取栈的最上面的元素，也就是取队列的最后一个元素
    //谁不为空就取谁
    if(!QueueEmpty(&obj->q1))
    {
        return QueueBack(&obj->q1);
    }
    else
    {
        return QueueBack(&obj->q2);
    }

}

/** Returns whether the stack is empty. */
bool myStackEmpty(MyStack* obj) 
{
    //两个队列都为空才为空
    return QueueEmpty(&obj->q1) && QueueEmpty(&obj->q2);
}

void myStackFree(MyStack* obj) 
{
    QueueDestory(&obj->q1);
    QueueDestory(&obj->q2);
    free(obj);
}

/**
 * Your MyStack struct will be instantiated and called as such:
 * MyStack* obj = myStackCreate();
 * myStackPush(obj, x);
 
 * int param_2 = myStackPop(obj);
 
 * int param_3 = myStackTop(obj);
 
 * bool param_4 = myStackEmpty(obj);
 
 * myStackFree(obj);
*/
```



