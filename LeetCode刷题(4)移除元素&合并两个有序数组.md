---
title: LeetCode刷题(4)移除元素&合并两个有序数组
date: 2021-06-05 16:03:54
tags: 
  - -LeetCode
  - -数组
  - -数据结构
  - -C语言
categories: 
  - 数据结构
cover: /img/leetcode.png
---

**移除元素**

典型双指针玩法。

[27. 移除元素 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/remove-element/)

***

我们都会想到这样的解法：从前面依次往后推，是val就将该数据后面的元素依次覆盖上来，但是这样的时间复杂度是O(n²)，最坏的结果是一个数组中大部分数据都是val。

所以我们想到另一种解法，以空间换时间 ，另开一个数组，把不是val的数据给新的数组，再把新数组的值拷贝回来。空间复杂度是O(n)。

但是这个题它不让开辟一个新的数组，所以我们还得换一个思路。

***

该思路空间复杂度为O(n),时间复杂度为O(1)。——**双指针解法**

定义两个指针，p1和p2，p1先动，p2后动，如果p1不等于val，就把值传给p2,直到完成一遍遍历，p2的值就是新数组元素的个数。

**如图所示：**

![image-20210605140450655](/images/LeetCode刷题(4)移除元素&合并两个有序数组.assets/image-20210605140450655.png)

![image-20210605140910200](/images/LeetCode刷题(4)移除元素&合并两个有序数组.assets/image-20210605140910200.png)

```c
int removeElement(int* nums, int numsSize, int val){
    int p1 = 0;
    int p2 = 0;
    //p1和p2都从数组左边出发
    while(p1 < numsSize)
    {
        //如果p1(对应的值)不等于val
        if(nums[p1] != val)
        {
            //将p1的值赋给p2
            nums[p2] = nums[p1];
            //往后面++
            p1++;
            p2++;
        }
        //p1(对应的值)等于val
        //只有p1走
        else
        {
            p1++;
        }
    }
    return p2;
}
```

就是p1在前面开路，p2在后面跟着，同时出发，p1遇到val就跳过，p2就停住,当p1没遇到val的时候将p1的值给p2，（就把p1位置的val值覆盖了）,然后p1，p2都往后走一位......

**合并两个有序数组**

[88. 合并两个有序数组 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/merge-sorted-array/)

![image-20210605142621467](/images/LeetCode刷题(4)移除元素&合并两个有序数组.assets/image-20210605142621467.png)

可以把num2直接放到num1后面，然后再进行升序排列，只不过效率有点低了。

所以我们采用下面这种解法。

num1和num2都从后往前走，取大的往后面放。

**如图所示：**

![image-20210605155039383](/images/LeetCode刷题(4)移除元素&合并两个有序数组.assets/image-20210605155039383.png)

![image-20210605155125941](/images/LeetCode刷题(4)移除元素&合并两个有序数组.assets/image-20210605155125941.png)

```c
void merge(int* nums1, int nums1Size, int m, int* nums2, int nums2Size, int n)
    //题目所给的nums1Size和num2Size没用
{
    int end1 = m-1;
    int end2 = n-1;
    int end = m+n-1;
    //end1和end2都还没有结束
    while(end1 >= 0 && end2 >= 0)
    {
        //把他们两个中大的放在后面
        if(nums1[end1] > nums2[end2])
        {
            nums1[end] = nums1[end1];
            end--;
            end1--;
        }
        else
        {
            nums1[end] = nums2[end2];
            end--;
            end2--;
        }
    }
    //如果end2先结束，就是num2里面已经没有元素了，那就不需要处理了，因为就是往num1里面放的
    //但是，如果是end1先结束了，还需要处理一下，因为此时num2里面还有元素没有放进num1里面
    while(end2 >= 0)
    {
        nums1[end] = nums2[end2];
        end--;
        end2--;
    }
}
```

