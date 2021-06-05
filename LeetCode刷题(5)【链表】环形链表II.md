---
title: LeetCode刷题(5)【链表】环形链表II
date: 2021-06-06 01:23:30
tags: 
  - -数据结构
  - -链表
  - -C语言
  - -LeetCode
categories:
cover: /img/leetcode.png
---

**环形链表I**

[LeetCode刷题(3)【链表】【环形链表】&扩展_半生瓜のblog-CSDN博客](https://blog.csdn.net/qq_51604330/article/details/117334723?spm=1001.2014.3001.5501)

***

**环形链表**II

[142. 环形链表 II - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

这个题写起来不难，但是证明有点麻烦。

![image-20210605184927049](/images/leetcode刷题-5.assets/image-20210605184927049.png)

![image-20210605184933830](/images/leetcode刷题-5.assets/image-20210605184933830.png)

***

针对这个入口点怎么求，有人给出了一个结论。

结论：一个指针从meet点开始走，一个指针从链表的开始点走，它们会在入口点相遇。（看下面的过程的时候，先别想这个结论，否则会越来越乱的，就先当不知道。）

***

![image-20210606005633219](/imagesleetcode刷题-5.assets/image-20210606005633219.png)

fast走的路程是slow走的路程的2倍。

slow走的路程：slow进环了以后，在一圈之内，fast一定会追上slow。因为slow走了一圈，fast都走两圈了。

slow进环之前，fast有可能在环里面转了N圈，如果入环之前的长度越长，环很小，N越大， 如果入环前的长度越短，环很大，N就是1，fast只转了1圈。

fast走的路程： L + C*N + X

slow走的路程：L + X

fast = 2*slow

L + C*N + X = 2(L +X)   

化简一下得：

C* N - X = L

再化简一下得：
(N-1)* C + C - X = L 

C - X就是meet点到入口点的距离。

再看这个结论。

**结论：一个指针从meet点开始走，一个指针从链表的开始点走，它们会在入口点相遇。**

理解一下，就是一个指针从meet点出发，转转转了N-1圈，在走了一个C-X到达入口点，发生相遇。

**代码实现：**

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode *detectCycle(struct ListNode *head) {
    struct ListNode* slow = head;
    struct ListNode* fast = head;
    while(fast && fast->next)
    {
        fast = fast->next->next;
        slow = slow->next;
        //找到相遇点
        if(fast == slow)
        {
            //相等即为相遇点
            struct ListNode* meet =  slow;
            //一个指针从meet走，一个指针从head走，他们会在入口点相遇
            while(head != meet)
            {
                head = head->next;
                meet = meet->next;
            }
            return meet;
        }
    }
    return NULL;
}
```











