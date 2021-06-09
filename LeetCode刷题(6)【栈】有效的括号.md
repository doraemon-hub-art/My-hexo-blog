---
title: LeetCode刷题(6)【栈】有效的括号
date: 2021-06-09 20:46:18
tags:
  - -C语言
  - -数据结构
  - -LeetCode
  - -栈
categorise:
cover: /img/leetcode.png
---

**有效的括号**

[20. 有效的括号 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/valid-parentheses/)

思路：是左括号，就入栈，是右括号，就与栈顶的左括号判断是否匹配，如果匹配，继续，不匹配就终止。

**从第79行开始，前面都是实现栈以及其功能接口。**

```C
typedef char StackDataType;
typedef struct Stack
{
    StackDataType* arry;
    int top;//指向栈顶
    int capacity;//栈的容量——能放几个数据
}Stack;
//初始化
void StackInit(Stack* ps)
{
    assert(ps);
    ps->arry = (StackDataType*)malloc(sizeof(StackDataType)*4);
    if (ps->arry == NULL)
    {
        printf("malloc fail");
        exit(-1);
    }
    ps->capacity = 4;
    ps->top = 0;
}
//销毁
void StackDestory(Stack* ps)
{
    assert(ps);
    free(ps->arry);
    ps->arry = NULL;
    ps->top = ps->capacity =0 ;
}
//入栈
void StackPush(Stack* ps, StackDataType x)
{
    assert(ps);
    //满了
    if (ps->top == ps->capacity)
    {
        StackDataType* tmp = (StackDataType*)realloc(ps->arry, ps->capacity * 2 * sizeof(StackDataType));
        if (tmp == NULL)
        {
            printf("realloc fail");
            exit(-1);
        }
        else
        {
            ps->arry = tmp;
            ps->capacity *= 2;
        }
    }
    ps->arry[ps->top] = x;
    ps->top++;
} 

//出栈
void StackPop(Stack* ps)
{
    assert(ps);
    //如果栈空了调用top，直接终止程序报错
    assert(ps->top > 0);
    ps->top--;
}
//返回栈顶元素
StackDataType StackTop(Stack* ps)
{
    assert(ps);
    assert(ps->top > 0);
    return ps->arry[ps->top - 1];
}
//返回栈中元素个数
int StackSize(Stack* ps)
{
    assert(ps);
    return ps->top;
}
//是否为空
bool StackEmpty(Stack* ps)
{
    assert(ps);
    return ps->top == 0;//真为空，假为非空。
}
///////////////////////////////////////////////////////////////////
bool isValid(char * s){
    Stack  st;
    StackInit(&st);
    while(*s != '\0')
    {
        switch(*s)
        {
            case '{':
            case '[':
            case '(':
            {
                StackPush(&st,*s);
                s++;
                break;
            }
            case '}':
            case ']':
            case ')':
            {
                if(StackEmpty(&st))
                {
                    StackDestory(&st);
                    return false;
                }

                char top = StackTop(&st);
                StackPop(&st);

                //不匹配的三种情况  
                if((*s == '}'&& top != '{')
                || (*s == ']'&& top != '[')
                || (*s == ')'&& top != '('))
                {
                  //不匹配返回fasle，有可能栈里面还有元素，销毁防止内存泄漏
                    StackDestory(&st);
                    return false;
                }
                else
                {
                    //匹配就继续匹配
                    s++;   
                }
                break;
            }
            default:
                break;
        }
    }
    bool ret = StackEmpty(&st);//匹配完成了，栈应该是空的。
    StackDestory(&st);
    return ret;
}
```

