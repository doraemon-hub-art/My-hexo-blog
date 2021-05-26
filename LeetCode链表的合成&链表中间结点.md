---
title: LeetCode链表的合成&链表中间结点
date: 2021-05-26 21:21:37
tags:
  - -C语言
  - -数据结构
  - -链表
categories: 
  - 数据结构
cover: 
---

**快慢指针问题：**

思路：定义一个快指针和一个慢指针，快指针走到结束的时候，慢指针刚好走到一半。

**链表的中间结点。**

[876. 链表的中间结点 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/middle-of-the-linked-list/)

```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */


struct ListNode* middleNode(struct ListNode* head){
    struct ListNode* slow = head;
    struct ListNode* fast = head;
    while(fast != NULL && fast->next != NULL)
    {
        slow = slow->next;
        fast = fast->next->next;
    }
    return slow;
}
```

**合并两个有有序链表：**

[21. 合并两个有序链表 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

思路：从头开始取两个链表中小的那个尾插到新链表。

```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */


struct ListNode* mergeTwoLists(struct ListNode* l1, struct ListNode* l2)
{
    //如果有一个链表是空的，那么直接返回另个一个链表
    if(l1 == NULL)
    {
        return l2;
    }
    if(l2 == NULL)
    {
        return l1;
    }
    //定义一个头指针head和尾指针tail
    struct ListNode* head = NULL;
    struct ListNode* tail = NULL;
    //如果来个两边都不是空链表进入迭代循环 
    while(l1 != NULL && l2 != NULL)
    {
        //如果取出来的值l1的小于l2的
        if(l1->val < l2->val)
        {
            //如果新链表是第一次插入
            if(tail == NULL)
            {
                //头尾指针都是l1的这一个元素
                head = tail = l1;
            }
            //如果新链表不是第一次插入
            else
            {
                //新链表的下一个结点是l1这个与元素
                tail->next = l1;
                //现在的尾巴是传入的这个元素
                tail = tail->next;
            }
            //链表l1的第一个元素往后推移一个
            l1 = l1->next;
        }
        //如果l1的第一个元素大于等于l2的第一个元素
        //下面同上
        else
        {
            if(tail == NULL)
            {
                head = tail = l2;
            }
            else
            {
                tail->next = l2;
                tail = tail->next;
            }
            l2 = l2->next;
        }
    }
    //循环结束
    //如果链表l1或者链表l2其中的一个还有元素，那么就直接插到后面
    if(l1 != NULL)
    {
        tail->next = l1;
    }
    if(l2 != NULL)
    {
        tail->next = l2;
    }
    return head;
}
```
