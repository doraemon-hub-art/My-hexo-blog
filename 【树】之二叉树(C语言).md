---
title: 【树】之二叉树(C语言)
date: 2021-06-16 12:57:39
tags:
  - -树
  - -二叉树
  - -数据结构
  - -C语言
categories:
  - 数据结构
cover: /img/realtree.png
---

# 树

## 树的概念及结构

## 树的概念

树是一种**非线性**的数据结构，它是由n(n >= 0)个有限结点组成的一个具有层次关系的集合，**把它叫做树是因为它看起来像一棵倒挂的树，也就是说它是根朝上，而叶朝下的**。

- 有一个特殊的结点，称为根结点，根节点没有前驱结点。
- 除跟根结点外，其余结点被分成M（M>0）个互不相交的集合T1、T2......Tm,其中每一个集合Ti(1<=i<=m)又是一棵结构与树类似的子树。每颗子树的根节点有且只有一个前驱，可以有0个或多个后继。
- 因此，树是递归定义的。

***

结点的度：到底有多少个链接的子节点

叶子结点或终端结点：度为0的结点称为叶子结点，

![image-20210612113428728](/images/树型结构.assets/image-20210612113428728.png)

- 节点的度:一个节点含有的子树的个数称为该节点的度;如上图:A的为6
- 叶节点或终端节点:度为0的节点称为叶节点;如上图:B、C、H、I..等节点为叶节点非终端节点或分支节点:度不为0的节点;如上图:D、E、F、G...等节点为分支节点
- 兄弟节点:具有相同父节点的节点互称为兄弟节点;如上图:B、C是兄弟节点
- 树的度:一棵树中，最大的节点的度称为树的度;如上图:树的度为6
- 节点的层次:从根开始定义起，根为第1层，根的子节点为第2层，以此类推;
- 树的高度或深度:树中节点的最大层次;如上图:树的高度为4（有两种说法-从0开始还是从1开始，空树-1，空树0）
- 节点的祖先:从根到该节点所经分支上的所有节点;如上图:A是所有节点的祖先
- 子孙:以某节点为根的子树中任一节点都称为该节点的子孙。如上图:所有节点都是A的子孙
- 森林:由m (m>0)棵互不相交的多颗树的集合称为森林;(数据结构中的学习并查集本质就是一个森林)——(日常很少碰到森林，并查集就是一个森林)

##  树的要求

- 子树是不相交的
- 除了根结点之外，每个结点有且仅有一个父结点
- 一个N个结点的树有N-1条边

## 树的表示

相对于线性表，树的结构就复杂很多了。最常用的表示方法——孩子兄弟表示法。

## 现实应用

文件系统的目录树，

树在实际当中，不太作为存储数据这个角度去用，因为意义不是很大。

主要用的是二叉树

# 二叉树

现实中的二叉树

这还是个满二叉树

![](/images/【树】之二叉树(C语言).assets/src=http___pic2.zhimg.com_v2-48ad88b651e76da3f8958831ba1cd80b_1200x500.jpg&refer=http___pic2.zhimg.jpg)

## 概念

与普通的树最大的不同是它最多只有两个子树。

## 特殊的二叉树

1. 满二叉树：每一层都是满的。

   假设一棵满二叉树的高度是 h,那么它的总结点个数是：2^0+2^1+2^2+......2^(h-1) =N。

   推导公式:2^h-1 = N;h = log2N+1以2位底N的对数+1。

   ![image-20210614201257487](/images/树型结构.assets/image-20210614201257487.png)

2. 完全二叉树

   完全二叉树是个效率很高的数据结构，完全二叉树是由满二叉树引出来的。

   假设树的高度是h,前h-1层是满的，最后一层不满，但是最后一层从左往右都是连续的。

   最后一层最少有一个结点。

   结点个数为:2^h-1-X= N,高度近似为:h = log2N+1+X以二为底N的对数+1

    ![image-20210614202340668](/images/树型结构.assets/image-20210614202340668.png)

## 注意

在实际中普通二叉树的增删查改没得意义。

**有意义的是搜索二叉树**。

**搜索二叉树**:任何一棵树，左子树都比跟要小，右子树都比根要大。在搜索树中查找一个数，最多查找高度次。时间复杂度O(N)。

引申：左右两边的结点数量比较均匀。

接着引出 ——**平衡树**

- AVL树
- 红黑树

学习普通二叉树可以为后面学习复杂的有用的平衡树做铺垫。

## 性质

1.若规定根结点的层数为1，则一棵非空二叉树的第i层上最多有2^(i-1)个结点

2.若规定根节点的层数是1，则深度为h的二叉树的最大节点数是2^-1

3.对于任何一棵二叉树，如果度为0其叶结点个数为n0,度为2的分支结点个数为n2,则有n0 = n2 +1（度为2的结点个数总是比度为0的结点个数多1）

4.若规定根节点的层数是1，具有n个结点的满二叉树的深度是h = log2 N +1（以2为底N的对数+1）

## 顺序存储

顺序结构存储就是使用数组来存储，一般使用数组只适合表示完全二叉树，因为不是完全二叉树会有空间的浪费。而现实中使用中只有堆才会使用数组来存储。二叉树顺序存储在物理上是一个数组，在逻辑上是一颗二叉树。

## 链式存储

二叉树的链式存储结构是指，用链表来表示一棵二叉树，即用链来指示元素的逻辑关系。通常的方法是链表中每个结点由三个域组成，数据域和左右指针域，左右指针分别用来给出该结点左孩子和右孩子所在的链结点的存储地址。链式结构又分为二叉链和三叉链，当前我们学习中一般都是二叉链，后面到高阶数据结构如红黑树等会用到三叉链。

## 构成&遍历



![image-20210613173854712](/images/树型结构.assets/image-20210613173854712.png)

任何一个二叉树由三个部分构成

1.根节点——2.左子树——3.右子树

 分治算法：分而治之，大问题分成子问题，子问题再分成子问题，直到无法分割

前序遍历：根左右——(上图：A-B-D-NULL-NULL-E-NULL-NULL-C-NULL-NULL)

中序遍历：左根右——(NULL-D-NULL-B-NULL-E-NULL-A-NULL-C-NULL)

后序遍历：左右根——(NULL-NULL-D-NULL-NULL-E-B-NULL-NULL-C-A) 

## 结构定义

```c
typedef char BinaryTreeType;
typedef struct BinaryTreeNode
{
	struct BinaryTreeNode* left;
	struct BinaryTreeNode* right;
	BinaryTreeType data;
}BTNode;
```

## 前序遍历

```c
void PrevOrder(BTNode* root)
{
	//如果树是空树就直接return
	if (root == NULL )
	{
		printf("NULL ");
		return;
	}
	printf("%c ", root->data);
	PrevOrder(root->left);
	PrevOrder(root->right);
}
```

## 中序遍历

```c
void InOrder(BTNode* root)
{
	if (root == NULL)
	{
		printf("NULL ");
		return;
	}
	InOrder(root->left);
	printf("%c ",root->data);
	InOrder(root->right);
}
```



## 后序遍历

```c
void PostOrder(BTNode* root)
{
	if (root == NULL)
	{
		printf("NULL ");
		return;
	}	
	PostOrder(root->left);
	PostOrder(root->right);
	printf("%c ", root->data);
}
```



## 构建一个简单的树

```c
//构建一个简单的树
	BTNode* A = (BTNode*)malloc(sizeof(BTNode));
	A->data = 'A';
	A->left = NULL;
	A->right = NULL;

	BTNode* B = (BTNode*)malloc(sizeof(BTNode));
	B->data = 'B';
	B->left = NULL;
	B->right = NULL;

	BTNode* C = (BTNode*)malloc(sizeof(BTNode));
	C->data = 'C';
	C->left = NULL;
	C->right = NULL;

	BTNode* D = (BTNode*)malloc(sizeof(BTNode));
	D->data = 'D';
	D->left = NULL;
	D->right = NULL;

	BTNode* E = (BTNode*)malloc(sizeof(BTNode));
	E->data = 'E';
	E->left = NULL;
	E->right = NULL;

	BTNode* F = (BTNode*)malloc(sizeof(BTNode));
	F->data = 'F';
	F->left = NULL;
	F->right = NULL;

	A->left = B;
	A->right = C;
	B->left = D;
	B->right = E;
	 
	PrevOrder(A);
	printf("\n");
```

![image-20210613202834074](/images/树型结构.assets/image-20210613202834074.png)

## 函数递归图——前序遍历

![](/images/树型结构.assets/无标题-1623589214513.png)

## 结点个数

**方法一**：因为我们要对同一个size进行++,所以要设置一个全局变量size进行++就完事了。

```c
int size = 0;
void TreeSize(BTNode* root)
{
	if (root == NULL)
	{
		return;
	}
	else
	{
		size++;
	}
	TreeSize(root->left);
	TreeSize(root->right);
}

```

但是在进行多次调用的时候因为累加，调用前要先将他置成0。

```c
TreeSize(A);
	printf("结点个数%d",size);
	size = 0;
	TreeSize(B);
	printf("结点个数%d", size);
```

但是这种方法不是线程安全的。引出我们的第二种方法。

**方法二**：传参

```c
int size = 0;
void TreeSize(BTNode* root,int* psize)
{
	if (root == NULL)
	{
		return;
	}
	else
	{
		(*psize)++;
	}
	TreeSize(root->left, psize);
	TreeSize(root->right, psize);
}
```

调用

```c
int Asize = 0;
	TreeSize(A,&Asize);
	printf("结点个数%d",Asize);
	size = 0;
	int Bsize = 0;
	TreeSize(B, &Bsize);
	printf("结点个数%d", Bsize);
```

**方法三**

**分治**

```c
int TreeSize(BTNode* root)
{
	return root == NULL ? 0:TreeSize(root->left) + TreeSize(root->right) + 1;
}
```

 调用

```c
printf("%d", TreeSize(A));
```

**图解**

![image-20210615194041323](/images/树型结构.assets/image-20210615194041323.png)

## 叶子结点的个数

  叶子结点没有子结点，叶子结点就是度为0的结点。

```c
int TreeLeafSize(BTNode* root)
{
	if (root == NULL)
	{
		return 0;
	}
	if (root->left == NULL && root->right == NULL)
	{
		return 1;
	}
	return TreeLeafSize(root->left) + TreeLeafSize(root->right);
}
```

流程：判断是否为空，为空返回0，不为空看左右孩子是否都为空，为空返回1，如果上面两个条件都不满足，分别计算左右孩子...孩子的孩子...递归返回相应的根结点。

![image-20210616113316365](/images/树型结构.assets/image-20210616113316365.png)

## 层序遍历

前序中序后序遍历其实也叫深度优先遍历。

层序遍历本质上也叫做广度优先遍历，以根为主一层一层往下遍历。

用队列先进先出的性质，

核心思路:上一层带下一层。

![](/images/树型结构.assets/叶子结点个数.png)

```c++
void LevelOrder(BTNode* root)
{
	Queue q;
	QueueInit(&q);
	if (root)//不为空进队 
	{
		QueuePush(&q, root);
	}
	while (!QueueEmpty(&q))//队列不为空就继续
	{
		BTNode* front = QueueFront(&q);
		QueuePop(&q);
		printf("%c ", front->data);
		if (front->left)
		{
			QueuePush(&q, front->left);

			if (front->right)
			{
				QueuePush(&q, front->right);
			}
		}
	}
	printf("\n");
	QueueDestory(&q);
}
```

![image-20210613173854712](/images/树型结构.assets/image-20210613173854712.png)

![image-20210616125449237](/images/树型结构.assets/image-20210616125449237.png)

## 全部代码

***

上面是队列

相关链接——[【线性表】之队列_半生瓜のblog-CSDN博客](https://blog.csdn.net/qq_51604330/article/details/117697458?spm=1001.2014.3001.5501)

***

```c
#include<stdio.h>
#include<stdlib.h>
#include<assert.h>
#include<stdlib.h>
#include<stdbool.h>
/// 队列
//前置声明
struct BinaryTreeNode;
typedef struct BinaryTreeNode* QueueDataType;//存二叉树的结点
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

/// //////////////////////////////////////////////////////

typedef char BinaryTreeType;
typedef struct BinaryTreeNode
{
	struct BinaryTreeNode* left;
	struct BinaryTreeNode* right;
	BinaryTreeType data;
}BTNode;
//二叉树不学习增删查改，因为没得意义
//前序遍历
void PrevOrder(BTNode* root)
{
	//如果树是空树就直接return
	if (root == NULL )
	{
		printf("NULL ");
		return;
	}
	printf("%c ", root->data);
	PrevOrder(root->left);
	PrevOrder(root->right);
}
//中序遍历
void InOrder(BTNode* root)
{
	if (root == NULL)
	{
		printf("NULL ");
		return;
	}
	InOrder(root->left);
	printf("%c ",root->data);
	InOrder(root->right);
}
//后序遍历
void PostOrder(BTNode* root)
{
	if (root == NULL)
	{
		printf("NULL ");
		return;
	}
	PostOrder(root->left);
	PostOrder(root->right);
	printf("%c ", root->data);
}
//结点的个数
/*因为我们要对同一个size进行++,
所以要设置一个全局变量size进行++就完事了,
但是在进行多次调用的时候因为累加，调用前要先将他置成0*/
//int size = 0;
//void TreeSize(BTNode* root)
//{
//	if (root == NULL)
//	{
//		return;
//	}
//	else
//	{
//		size++;
//	}
//	TreeSize(root->left);
//	TreeSize(root->right);
//}
//传参
//int size = 0;
//void TreeSize(BTNode* root,int* psize)
//{
//	if (root == NULL)
//	{
//		return;
//	}
//	else
//	{
//		(*psize)++;
//	}
//	TreeSize(root->left, psize);
//	TreeSize(root->right, psize);
//}
int TreeSize(BTNode* root)
{
	return root == NULL ? 0:TreeSize(root->left) + TreeSize(root->right) + 1;
}
//叶子结点的个数
int TreeLeafSize(BTNode* root)
{
	if (root == NULL)
	{
		return 0;
	}
	if (root->left == NULL && root->right == NULL)
	{
		return 1;
	}
	return TreeLeafSize(root->left) + TreeLeafSize(root->right);
}

//层序遍历
void LevelOrder(BTNode* root)
{
	Queue q;
	QueueInit(&q);
	if (root)//不为空进队 
	{
		QueuePush(&q, root);
	}
	while (!QueueEmpty(&q))//队列不为空就继续
	{
		BTNode* front = QueueFront(&q);
		QueuePop(&q);
		printf("%c ", front->data);
		if (front->left)
		{
			QueuePush(&q, front->left);

			if (front->right)
			{
				QueuePush(&q, front->right);
			}
		}
	}
	printf("\n");
	QueueDestory(&q);
}

int main(void)
{
	//构建一个简单的树
	BTNode* A = (BTNode*)malloc(sizeof(BTNode));
	A->data = 'A';
	A->left = NULL;
	A->right = NULL;

	BTNode* B = (BTNode*)malloc(sizeof(BTNode));
	B->data = 'B';
	B->left = NULL;
	B->right = NULL;

	BTNode* C = (BTNode*)malloc(sizeof(BTNode));
	C->data = 'C';
	C->left = NULL;
	C->right = NULL;

	BTNode* D = (BTNode*)malloc(sizeof(BTNode));
	D->data = 'D';
	D->left = NULL;
	D->right = NULL;

	BTNode* E = (BTNode*)malloc(sizeof(BTNode));
	E->data = 'E';
	E->left = NULL;
	E->right = NULL;

	BTNode* F = (BTNode*)malloc(sizeof(BTNode));
	F->data = 'F';
	F->left = NULL;
	F->right = NULL;

	A->left = B;
	A->right = C;
	B->left = D;
	B->right = E;
	 
	LevelOrder(A);


	printf("\n");
	return 0;
}
```

