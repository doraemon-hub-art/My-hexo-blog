---
title: LeetCode刷题(10)【简单】反转整数(C++)
date: 2021-08-16 23:23:34
tags:
  - -C++
  - -LeetCode
categories:
  - C++
cover: /img/leetcode.png
---

**题目链接**:——[反转整数](https://leetcode-cn.com/problems/reverse-integer/submissions/)
![在这里插入图片描述](https://img-blog.csdnimg.cn/16eb619dbed94644b5fbfa5fa6a32f82.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzUxNjA0MzMw,size_16,color_FFFFFF,t_70)
**代码示例**:

```c++
class Solution {
public:
    int reverse(int x)
    {
        int ret =0;
       while(x)
       {
       //有符号整数溢出
       
       //如果这个数比最小的数去掉一位要小，或者比最大的数去掉一位要大
       //那么将他*10后得到的最后结果肯定是要大(小)，肯定溢出了。
       //并且要先在*10之前判断，否则就溢出了
          	if (ret > INT_MAX / 10 || ret < INT_MIN / 10)
			{
				return 0;
			}
           ret = ret*10+x%10;        
           x /=10;
       }
        return ret;
    }
};
```
**题解**:
```c++
		INT_MAX和INT_MIN为C++内宏定义,分别表示int的最大值和最小值。

  		定义ret为反转后的数，初始化为0
          x%10取到最后一位上的数
          x/10去掉最后一位上的数
	
	相关解释：

		开始的ret为0
		x %10将原来x的最后一位取出来，放到ret中，这个数就是ret最终结果的第一位。
		现在的ret是这个一位数，
		
		将它*10，变成几十，ret变成两位数，刚才取出来的这个数到了十位上，个位上是0，
		个位就被空了出来，
		之前的x已经被去掉了最后一位，现在的x最后一位为原来x的倒数第二位，x %10,取到新的最后一位，
		加到ret中，得到新的ret
		重复上述步骤:
		...
		

		例: x = 123
		ret =  0 * 10 + 123 % 10 = 3 
		x = 12
		
		ret = 3 * 10 + 12 % 10 = 30 + 2 = 32
		x = 1
	
		得到结果
		ret = 32 * 10 + 1 % 10  = 321


```
