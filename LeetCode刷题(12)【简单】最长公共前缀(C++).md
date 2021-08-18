---
title: LeetCode刷题12
date: 2021-08-18 15:25:10
tags:
  - -C++
  - -算法
  - -LeetCode
categories:
  - -算法
cover: /img/leetcode.png
---

#  最长公共前缀
**题目链接**——[最长公共前缀](https://leetcode-cn.com/problems/longest-common-prefix/)
![在这里插入图片描述](https://img-blog.csdnimg.cn/db1c4c1e85e14b32ab39234dad7b2765.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzUxNjA0MzMw,size_16,color_FFFFFF,t_70)


**代码示例**:
```c
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
       //容器为空
       if(strs.size() == 0)
       {
           return "";
       }
       
       for (int i = 0; i < strs[0].size(); i++) 
       {
            char ch = strs[0][i];
            for (int j = 1; j < strs.size();j++) 
            {
                if (strs[j][i] != ch || i > strs[j].size()) 
                {
                    return strs[0].substr(0, i);
                }
            }
        }
        //全都一样
        return strs[0];
    }
};
```
**题解**:
```c
垂直比较。
如果容器为空，返回“”

不为空
以容器中第一个字符串为标准，将它的每个字母和容器中其它字符串的每一个字母做比较，
如果不同或者此时遍历的长度i，已经大于了其他某个字符串的长度，
那么直接返回第一个字符串截取到上一个i,这么长。
substr截取区间为左闭右开。

容器中字符串全都相等，或者只有一个元素
返回本身（第一个字符串）。
```
