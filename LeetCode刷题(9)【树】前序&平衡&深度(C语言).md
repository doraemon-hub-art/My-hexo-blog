---
title: LeetCode刷题(9)【树】前序&平衡&深度
date: 2021-06-18 09:06:50
tags:
  - -数据结构
  - -LeetCode
  - -C语言
  - -树
  - -二叉树
categories: 
  - 数据结构
cover: /img/leetcode.png
---

**二叉树知识回顾**——[【树】之二叉树(C语言)(含图解)_半生瓜のblog-CSDN博客](https://blog.csdn.net/qq_51604330/article/details/117955503?spm=1001.2014.3001.5501)

**二叉树的前序遍历**

[144. 二叉树的前序遍历 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

本题中，对于C++或者Java等语言，返回的是它们的数据结构库里面的数据结构，而C语言没有，这也就是如果用C语言往后通吃数据结构会困难的原因。

注意本体的传参，操作的是不是一个数。

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */


/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
 //计算结点个数
 int TreeSize(struct TreeNode* root)
 {
     return root == NULL?0:TreeSize(root->left)+TreeSize(root->right)+1;
 }
 //前序遍历
void _preorder(struct TreeNode* root,int* a,int *i)//为了保证一直对一个i进行操作所以要传地址
{
    if(root == NULL)
    {
        return ;
    }
    a[*i] = root->val;
    (*i)++;
    _preorder(root->left,a,i);
    _preorder(root->right,a,i);
}
int* preorderTraversal(struct TreeNode* root, int* returnSize){
     int size = TreeSize(root);
     //创建数组
     int* a = (int*)malloc(size*sizeof(int));
     int i = 0;
     _preorder(root,a,&i);
     *returnSize = size;
     return a;
}
```

**二叉树的最大深度**

**经典的分治问题**

[104. 二叉树的最大深度 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

一棵树的高度就是最长路径的结点个数。

- 空 -  高度为0
- 非空 左右子树深度大的内个+1

本质上用的后序遍历，先求左，后求右边，再求自己。

**图示**

![image-20210618084730107](/images/LeetCode刷题(9)【树】前序&平衡&深度.assets/image-20210618084730107.png)

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */


int maxDepth(struct TreeNode* root){
    if(root == NULL)
        return 0;
    int leftDepth = maxDepth(root->left);
    int rightDepth = maxDepth(root->right);

    return leftDepth > rightDepth?leftDepth+1:rightDepth+1;
}
```

**平衡二叉树**

[Loading Question... - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/balanced-binary-tree/)

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */
int maxDepth(struct TreeNode* root){
    if(root == NULL)
        return 0;
    int leftDepth = maxDepth(root->left);
    int rightDepth = maxDepth(root->right);

    return leftDepth > rightDepth?leftDepth+1:rightDepth+1;
}

bool isBalanced(struct TreeNode* root){
    //空树也满足条件
    if(root == NULL)
    {
        return true;
    }
    int leftDepth = maxDepth(root->left);
    int rightDepth = maxDepth(root->right);
    //如果一开始就不满足就没必要往下进行了，满足就递归判断左右
    return abs(leftDepth-rightDepth) < 2
    && isBalanced(root->left)
    && isBalanced(root->right);                                          
}
```

