---
title: NowCoder刷题(1)【树】二叉树的遍历
date: 2021-06-21 20:57:31
tags:
  - -数据结构
  - -C语言
  -	-树
  - -二叉树
  - -NowCoder
categories:	
  - -NowCoder
cover: 
---

**二叉树的遍历(IO型)**

[二叉树遍历_牛客题霸_牛客网 (nowcoder.com)](https://www.nowcoder.com/practice/4b91205483694f449f94c179883c1fef?tpId=60&&tqId=29483&rp=1&ru=/activity/oj&qru=/ta/tsing-kaoyan/question-ranking)

**题目描述**

![image-20210621205635444](/images/NowCoder刷题(1)【树】二叉树的遍历.assets/image-20210621205635444.png)

如图所示的这棵树

![image-20210620204206164](/images/NowCoder刷题(1)【树】二叉树的遍历.assets/image-20210620204206164.png)

前序输出结果为

A-B-D-#-#-E-#-#-C-#-#

还原过程

**示例1**

![image-20210621191417552](/images/NowCoder刷题(1)【树】二叉树的遍历.assets/image-20210621191417552.png)

**示例2**

——前序遍历还原

![image-20210621192508286](/images/NowCoder刷题(1)【树】二叉树的遍历.assets/image-20210621192508286.png)

**代码实现**：

```c
#include<stdio.h>
//定义一棵树的结构
typedef struct TreeNode
{
	struct TreeNode* left;
	struct TreeNode* right;
	char val;
}TreeNode;
//根据前序遍历还原这棵树
TreeNode* CreatBackTree(char* a, int* i)
{
	if (a[*i] == '#')
	{
		//井号说明该结点为空
		++(*i);
        return NULL;
	}
	TreeNode* root = (TreeNode*)malloc(sizeof(TreeNode));
	if (root == NULL)
	{
		printf("malloc is fail");
		exit(-1);
	}
	root->val = a[*i];
	(*i)++;
	root->left = CreatBackTree(a, i);//构建子树的时候还是从这个结点开始
	root->right = CreatBackTree(a, i);
	return root;
} 
//中序遍历
void InOrderTree(TreeNode* root)
{
	if (root == NULL)
	{
		return;
	}
	InOrderTree(root->left);
	printf("%c ", root->val);
	InOrderTree(root->right);
}
int main(void)
{
	//输入一个字符数组
	char arry[100];
	scanf("%s", arry);//输入字符串不用取地址符
	int i = 0;
	TreeNode* root = CreatBackTree(arry, &i);
	InOrderTree(root);
	return 0;
}
```

![](/images/NowCoder刷题(1)【树】二叉树的遍历.assets/二叉树遍历22-1624279920068.png)

