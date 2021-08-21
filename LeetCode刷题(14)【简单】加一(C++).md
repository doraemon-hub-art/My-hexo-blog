---
title: LeetCode刷题(14)【简单】加一(C++)
date: 2021-08-21 12:51:17
tags:
  - -LeetCode
  - -C++
  - -算法
categories:
  - 算法
cover: /img/leetcode.png
---

# 加一
**题目链接**——[加一](https://leetcode-cn.com/problems/plus-one/solution/)
![在这里插入图片描述](https://img-blog.csdnimg.cn/36730e8edad74cb8b8f2fee4212a6ef1.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzUxNjA0MzMw,size_16,color_FFFFFF,t_70)**代码示例**：
```c++
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
  
  //从最后一位开始
    for(int i = digits.size()-1;i>= 0;i--)
    {
    //最后一位不是9,+1直接return
    //如果此时后一位由9已经变成了0
    //紧着这判断这位不是9，+1直接return,就相当于满10进1
        if(digits[i]+1 != 10)
        {
            digits[i] += 1;
            return digits;
        }
        
        //最后一位加上1等于10
        //变成0
        digits[i] =0;
                                        
    }

    //原来容器中全都是9
    digits[0] =1;
    digits.push_back(0);
    return digits;

    }
};
```

**错误示例——越界**
```c++
将容器中数取出来编程对应的一个数，加1然后在求得每位上的数再存到容器中，返回该容器，
但是当原来容器中数过多时，先求出来的数会发生溢出，大于INT_MAX。
所以错误。


class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
   
    int temp1 = 0;
    int temp2 = 0;
    int n =1;
    int ret = 0;
    for(int i = 0;i<digits.size();i++)
    {
        for(int j =digits.size()-1;j>i;j--)
        {
            n = n*10;
        }
        temp1 =  digits[i] * n;
        ret += temp1;
        n = 1;
    }
    ret +=1;

     digits.clear();

    while(ret)
    {
        temp2 = ret%10;
        digits.push_back(temp2);
        ret /= 10;
    }

    vector<int>Tempdigits;

    while(!digits.empty())
    {
        Tempdigits.push_back(digits.back());
        digits.pop_back();
    }

    return  Tempdigits;
    }
};
```
