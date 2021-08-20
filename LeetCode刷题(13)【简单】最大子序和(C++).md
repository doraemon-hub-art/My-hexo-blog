---
title: LeetCode刷题(13)【简单】最大子序和(C++)
date: 2021-08-20 19:46:53
tags:
  - -LeetCode
  - -算法
  - -C++
categories:
  - 算法
cover: /img/leetcode.png
---

# 最大子序和
**题目链接**——[最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

![在这里插入图片描述](https://img-blog.csdnimg.cn/c8d97260a62f40cdb6542f7dff1c121a.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzUxNjA0MzMw,size_16,color_FFFFFF,t_70)**代码示例**:
```c++
最笨的方法:
	依次从每个元素开始往后一个一个的相加，加到temp1中，如果比之前的大就存到temp2中，最后得到最大的和。
	每轮完重置temp1

class Solution {
public:
    int maxSubArray(vector<int>& nums) {   
    int temp1 = 0;
    int temp2 = INT_MIN;
    for(int i =0;i<nums.size();i++)
    {
        for(int j =i;j <nums.size();j++)
        {
            temp1  += nums[j];
           
            if(temp1 >= temp2)
            {
                temp2 = temp1;
            }
        }
        temp1 = 0;
    }
    return temp2;
    }
};
```
