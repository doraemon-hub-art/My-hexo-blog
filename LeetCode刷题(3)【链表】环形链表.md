**环形链表**

[141. 环形链表 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/linked-list-cycle/)

什么是链表带环：链表的最后一个元素不指向空而指向前面的某个结点。

思路：**快慢指针**，慢指针走一步，快指针走两步，二者先后 进入环内进行追逐，最终会在某个点相遇。

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
bool hasCycle(struct ListNode *head) {
    struct ListNode* slow = head;
    struct ListNode* fast = head;
    while(fast && fast->next)
    {
        slow = slow->next;
        fast = fast->next->next;
        if(slow == fast)
            return true;
    }
    return false;
}
```

**扩展：**

请证明：

**(1)**slow和fast一定会在环里面相遇呢？有没有可能永远追不上？

当slow 走1步，fast走2步时，**一定可以**追上。

若slow和fast已经进入环中，追逐已经开始了，假设他们之间的距离是N,slow走1步，fast走2步，二者的距离每次缩减1，N,N-1,N-2,......0,直到相遇。

**(2)**slow一次走1步，fast一次走3不行不行？4不行不行？	

**不一定可以追上，甚至有可能会进入死循环。**我比你快不一定追上，因为存在错过。若开始追逐，假设二者距离为N，假设slow走1步，fast走3步，距离每次缩减2，N,N-2,N-4,N-6......。如果N是偶数最后会减到0，如果N是偶数则减到-1，距离为0代表相遇，距离为-1代表反超了，进入新的追逐，他们之间的距离是 C-1(假设C 是环的长度)，如果C-1是偶数，就可以追上，如果C-1是奇数，就永远追不上，因为是奇数的时候又像开始那样反超，距离又是C-1，就永远追不上。

其他fast步数同理分析。
