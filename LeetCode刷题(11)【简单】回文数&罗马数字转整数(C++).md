---
title: LeetCode刷题(11)【简单】回文数&罗马数字转整数(C++)
date: 2021-08-17 19:49:02
tags:
  - -C++
  - -LeetCode
  - -算法
categories:
  - 算法
cover: /img/leetcode.png
---

# 回文数
**题目链接**——[回文数](https://leetcode-cn.com/problems/palindrome-number/)
![在这里插入图片描述](https://img-blog.csdnimg.cn/5eb45e69583646b5ae25cf45da829c73.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzUxNjA0MzMw,size_16,color_FFFFFF,t_70)

**代码示例**：

```c
class Solution {
public:
    bool isPalindrome(int x) {
    if(x < 0)
    {
        return false;
    }
    else
    {
        int ret = 0;
        int temp = x;
        while(temp)
        {
            if (ret > INT_MAX / 10 || ret < INT_MIN / 10)
			{
				return 0;
			}
            ret = ret*10 + temp% 10;
            temp /= 10;
        }
        if(ret == x)
        {
            return true;
        }
        else
        {
            return false;
        }
    }
    }
};

```



**题解**:

**反转整数**——[反转整数](http://doraemon2.xyz/2021/08/16/LeetCode%E5%88%B7%E9%A2%98(10)%E3%80%90%E7%AE%80%E5%8D%95%E3%80%91%E5%8F%8D%E8%BD%AC%E6%95%B4%E6%95%B0(C++)/)

```c
同反转整数，在此基础上定义临时变量，不要更改原来的x。
```

# 罗马数字转整数
**题目链接**——[罗马数字转整数](https://leetcode-cn.com/problems/roman-to-integer/)
![在这里插入图片描述](https://img-blog.csdnimg.cn/4288a5cc564443ee81cbc04f7a950e37.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzUxNjA0MzMw,size_16,color_FFFFFF,t_70)**代码示例**:
```c
class Solution {
public:

int GetNum(char ch)
    {
        switch(ch)
        {          
        case 'I':
            return 1;
        case 'V':    
            return 5;
        case 'X':
            return 10;
        case 'L':
            return 50;
        case 'C':
            return 100;
        case 'D':
            return 500;
        case 'M':
            return 1000;
        default:
            return 0;
        }
    }

    int romanToInt(string s) {
        int ret = 0;
        int num =0;
        int nextnum = 0;
        for(int i =0;i<s.size();i++)
        {
            num = GetNum(s[i]);
            if(i == s.size()-1)
            {
                ret += num;
            }
            else
            {
                nextnum = GetNum(s[i+1]);
                if(num<nextnum)
                {
            
                    ret -= num;
                }
                else
                {
                    ret += num;
                }
            }
           
        }
        return ret;
    }


};
```
**题解**:
```c
定义ret为最后的结果

通过观察罗马数字，得到规律，多个字母拼接的罗马数字，
从左到右依次取每个字母，得到对应的数值，和挨着的下一个字母对应的数值，
如果当前字母对应数值小于下一个字母对应的数字，
那么当前字母对应的数值就变成负的，反之不做改变，
不断加到ret中。
......
其中，到了最后一个字母就不找下一个字母了，直接将它对应的数值加到ret中，
返回结果ret。
```

