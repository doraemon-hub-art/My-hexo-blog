---
title: LeetCode反转链表
date: 2021-05-23 15:25:29
tags:
  - -数据结构
  - -链表
categories:
  - 数据结构
cover: /img/fanzhuanlianbiao.png
---

***

题目链接——[206. 反转链表 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/reverse-linked-list/submissions/)**

***

**反转链表**

**思路一:反转指针**。

![image-20210523125459086](/images/LeetCode反转链表.assets/image-20210523125459086.png)

本质上就是调转指针的方向。

首先我们定义两个指针,一个叫n1，一个叫n2。(Node1,Node2)

![image-20210523125823700](/images/LeetCode反转链表.assets/image-20210523125823700.png)

让n2指向第一个结点，让n1指向空。

![image-20210523125932679](/images/LeetCode反转链表.assets/image-20210523125932679.png)

n2->next指向n1。

![image-20210523130017978](/images/LeetCode反转链表.assets/image-20210523130017978.png)

但是，两个指针是反不转的。因为：

这里让n2->next指向n1，就是把n1的值存到n2的next上，n2->next原来存的是2的地址，现在存的是NULL，但是继续往后走的时候，我们发现找不到2了 。

所以要反转指针，两个指针是反不动的，要用3个。

前两个指针 反转，最后一个指针负责记录下一个位置。

![image-20210523130643211](/images/LeetCode反转链表.assets/image-20210523130643211.png)

![image-20210523131751386](/images/LeetCode反转链表.assets/image-20210523131751386.png)

![image-20210523131821728](/images/LeetCode反转链表.assets/image-20210523131821728.png)

![image-20210523131916171](/images/LeetCode反转链表.assets/image-20210523131916171.png)

什么时候结束

n2 == NULL；

***

重复的条件用循环解决

1. 初始条件
2. 迭代过程
3. 结束条件

画图看起来很浪费时间，但提升了写代码的体验，更好的解决问题。

***

**代码实现：**

```C
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */

struct ListNode* reverseList(struct ListNode* head){
    if(head == NULL)
    {
        return NULL;
    }
    //初始化条件
    struct ListNode* n1 = NULL,*n2 = head,*n3 = n2->next;
    //结束条件
    while(n2 != NULL)
    {
        //迭代过程
        n2->next = n1;
        //往后推移
        //两个相等就是往后推移
        n1 = n2;
        n2 = n3;
        if(n3 != NULL)
        {   
            n3 = n3->next;
        }
    }
    return n1;
}   
```

**思路二：头插法**

取结点头插到新链表中。cur是当前操作结点，用一个next来保存下一个结点(同上)。

文字简单描述：

​	从原链表去一个点下来，放到新的链表中，当做新链表的头结点cur = newhead,

迭代往后走，取下一个结点......

**代码实现：**

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */

struct ListNode* reverseList(struct ListNode* head){
   struct ListNode* cur = head;
   struct ListNode* newHead = NULL;
   while(cur != NULL)
   {
       struct ListNode* next = cur->next;
        //头插法
        cur->next =newHead;
        newHead = cur;
        cur = next;
   }
   return newHead;
}   
```



