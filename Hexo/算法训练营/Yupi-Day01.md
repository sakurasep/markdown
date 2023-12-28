---
title: 算法通关村第一关-链表青铜挑战笔记
tags: [学习笔记,鱼皮的算法通关村]
categories: [学习笔记]
description: 记录自己的算法学习日常
references:
  - title: 九月的生活
    url: https://blog.sakurasep.site/
mathjax: false
mermaid: false
date: 2023-09-30 16:30:46
cover: miku
banner: https://cdn.sakurasep.site/Wallpaper/10.jpg
abbrlink: Yupi-Day01
Hidden: false
---

<!-- more -->

{% quot 链表青铜挑战 Day1 Part1 %}

# 前言

本章节是关于单链表的增加和删除操作,根据题目要求把需要用到的函数都进行了自定义,方便重复调用

大部分的思路都以注释的方式展现,因为比较忙,就不放图了,效率第一!

希望各位可以指出我文章中的错误,因为个人的理解难免会出现差错,感谢!

# 更新日志

{% timeline %}

<!-- node 2023年9月10日 -->

完成任务要求

<!-- node 2023年9月10日 -->

完成了笔记的大部分内容,程序已经可以执行,还需要完善边界条件

{% endtimeline %}

## 通关要求

{% checkbox symbol:plus color:green checked:true 1.理解C语言里是如何构造出链表的 %}
{% checkbox symbol:plus color:green checked:true 2.链表增加元素，首部、中间和尾部分别会有什么问题，该如何处理？ %}
{% checkbox symbol:plus color:green checked:true 3.链表删除元素，首部、中间和尾部分别会有什么问题，该如何处理？ %}


## 代码思路

### 链表的定义

在链表的每一个结点都是由数据域和指针域构成,数据域可以有不同的数据类型

这种依靠指针的数据结构优点就是添加和删除操作比较简单,缺点也显而易见,由于数据是分散存储,导致访问其中某一个数据必须对链表进行遍历

{% folding child:codeblock open:false color:dark 链表的定义 %}
```cpp
struct ListNode{
  int num; // 数值
  struct ListNode *next; // 指针
};
```
{% endfolding %}

### 链表的初始化

当我们对链表进行初始化的时候,首先应该新建一个头指针,并将指针初始化为NULL

然后创建头结点s,初始化它的指针为NULL.此时让头指针指向s,此时,一个最基础的链表诞生了

接下来就是不断创建新的结点,每一个新的结点都会成为链表的尾部,然后将头结点s的指针指向新创建的结点,将结点加入链表.

最后就是将头结点的位置向后移动,以便于下一次将新结点加入链表

{% folding child:codeblock open:false color:dark 链表的初始化 %}
```cpp
// 初始化链表
struct ListNode* initList()
{
  // 1.创建头指针
  struct ListNode *p = NULL;
  // 2.创建头结点
  struct ListNode *s = (struct ListNode *)malloc(sizeof(struct ListNode));
  s->next = NULL;
  p = s; // 头指针指向头结点
  // 3.新建结点使得每一个加入的数值保存在新结点,并加入链表
  for(int i = 1; i < 5; i++)
  {
    // 创建新结点a,给a赋值,并且指针指向链表尾部
    struct ListNode *a = (struct ListNode *)malloc(sizeof(struct ListNode));
    a->num = i;
    a->next = NULL;
    // 首先将头结点指向这个新建立的结点,此时这个结点已经加入了链表
    s->next = a;
    // 然后头结点会相当于移动到a这个节点,此时头结点指针指向NULL,准备下一次的a结点加入链表
    s = s->next;
  }
  // 返回整个链表的头指针,便于遍历
  return p;
}
```
{% endfolding %}

### 链表的遍历

链表的不便就在于获取链表的长度或是访问链表某一位置的数据,都需要对链表进行遍历,最佳情况肯定是数据在表头,最糟糕的情况就是数据在表尾

对于遍历链表,最重要的是知道什么时候停止遍历.单链表的表尾永远指向NULL,所以我们当我们从头开始遍历,直到结点指针指向NULL就停止,此时停留在表尾结点

{% folding child:codeblock open:false color:dark 链表的遍历 %}
```cpp
// 获取链表长度
int getListLength(struct ListNode *p)
{
  int length = 0;
  struct ListNode *s = p;
  while (s->next != NULL)
  {
    length++;
    s = s->next;
  }
  return length;
}
```
{% endfolding %}


### 链表结点的增加

1. 表头插入元素
这里我们首先要获取到头指针,先将新结点的指针指向头指针的指向的结点,然后将头指针指向新结点,此时就完成了表头插入元素的操作,当然不要忘了数值域

2. 表尾插入元素
这里我们先要遍历链表获取到表尾结点,然后将表尾结点的指针指向新结点,此时就完成了表尾元素的插入

3. 插入中间位置
举个例子,如果我要在2这个位置插入元素,那么先遍历到结点1,然后新建一个结点,将新结点的指针指向结点1所指向的结点2,然后将结点1的指针指向新结点,此时新结点变成了结点2,之前的结点2变成了结点3

{% folding child:codeblock open:false color:dark 链表结点的增加 %}
```cpp
// 添加元素 函数传入的是头指针,操作数,插入数值
// 1.头插法 2.尾插法 3.插入中间位置
bool addNode(struct ListNode *p, int op, int number)
{
  // 考虑此时为空链表
  if(p == NULL)
  {
    return false;
  }
  // 头插法
  if(op == 1)
  {
    struct ListNode *s = (struct ListNode *)malloc(sizeof(struct ListNode));
    s->num = number;
    s->next = p->next;
    p->next = s;
  }
  // 尾插法
  if(op == 2)
  {
    struct ListNode *s = (struct ListNode *)malloc(sizeof(struct ListNode));
    s->next = p->next;
    while (s->next != NULL)
    {
      s = s->next;
    }
    struct ListNode *a = (struct ListNode *)malloc(sizeof(struct ListNode));
    s->next = a;
    a->num = number;
    a->next = NULL;
  }
  // 插入到中间位置
  if(op == 3)
  {
    struct ListNode *s = p;
    int position = 0; // 插入位置
    printf("请输入要插入的位置,只可以插入1-%d:\n", getListLength(p)-1);
    scanf("%d", &position);
    // 考虑边界
    if(position < 1 || position >= getListLength(p))
    {
      return false;
    }
    for (int i = 1; i <= position; i++)
    {
      s = s->next;
    }
    struct ListNode *a = (struct ListNode *)malloc(sizeof(struct ListNode));
    a->next = s->next;
    a->num = number;
    s->next = a;
  }
  return true;
}
```
{% endfolding %}

### 链表结点的删除

1. 删除头部元素
因为我们知道头指针,所以很简单,只需要让头指针指向当前结点的下一个结点即可,此时表头将会变成结点2,结点1并没有被删除,而是被隐藏

2. 删除尾部元素
首先要遍历整个链表,使得找到指向表尾元素的指针,也就是找到倒数第二个结点,将该结点的指针指向NULL,此时表尾结点被隐藏

3. 删除中间元素
举个例子,我要删除位置2的结点,那么我找到位置1的结点,然后将该结点的指针指向位置3,也就是位置2结点的指针,此时结点2就不会有结点指向,相当于被隐藏

{% folding child:codeblock open:false color:dark 链表结点的删除 %}
```cpp
// 删除元素 函数传入的是头指针,操作数
// 1.删除头部元素 2.删除尾部元素 3.删除中间元素
bool deleteNode(struct ListNode *p, int op)
{
  // 考虑空链表
  if(p == NULL)
  {
    return false;
  }
  // 删除头部元素
  if(op == 1)
  {
    p->next = p->next->next;
  }
  // 删除尾部元素
  if(op == 2)
  {
    struct ListNode *s = p;
    while(s->next->next != NULL)
    {
      s = s->next;
    }
    s->next = NULL;
  }
  if(op == 3)
  {
    struct ListNode *s = p;
    int position = 0;
    printf("请输入要删除结点的位置,当前链表总长度为%d:\n", getListLength(p));
    scanf("%d", &position);
    // 考虑边界情况
    if(position < 1 || position > getListLength(p))
    {
      return false;
    }
    for(int i = 1; i < position; i++)
    {
      s = s->next;
    }
    s->next = s->next->next;
  }
  return true;
}
```
{% endfolding %}

## 运行截图

{% swiper effect:cards %}
![](https://cdn.sakurasep.site/Article/Yupi/Day1/output1.png?imageSlim)
![](https://cdn.sakurasep.site/Article/Yupi/Day1/output2.png?imageSlim)
![](https://cdn.sakurasep.site/Article/Yupi/Day1/output3.png?imageSlim)
{% endswiper %}

## 完整运行代码

{% folding child:codeblock open:false color:dark 完整代码 %}
```cpp
/*
作者: 上杉九月
题目: 算法通关村第一关-链表青铜挑战笔记
时间: 2023.09.02
*/
#include <iostream>
using namespace std;

// 链表的定义
struct ListNode{
  int num;
  struct ListNode *next; // 指针
};

// 初始化链表
struct ListNode* initList()
{
  // 1.创建头指针
  struct ListNode *p = NULL;
  // 2.创建头结点
  struct ListNode *s = (struct ListNode *)malloc(sizeof(struct ListNode));
  s->next = NULL;
  p = s; // 头指针指向头结点
  // 3.新建结点使得每一个加入的数值保存在新结点,并加入链表
  for(int i = 1; i < 5; i++)
  {
    // 创建新结点a,给a赋值,并且指针指向链表尾部
    struct ListNode *a = (struct ListNode *)malloc(sizeof(struct ListNode));
    a->num = i;
    a->next = NULL;
    // 首先将头结点指向这个新建立的结点,此时这个结点已经加入了链表
    s->next = a;
    // 然后头结点会相当于移动到a这个节点,此时头结点指针指向NULL,准备下一次的a结点加入链表
    s = s->next;
  }
  // 返回整个链表的头指针,便于遍历
  return p;
}

// 获取链表长度
int getListLength(struct ListNode *p)
{
  int length = 0;
  struct ListNode *s = p;
  while (s->next != NULL)
  {
    length++;
    s = s->next;
  }
  return length;
}

// 添加元素 函数传入的是头指针,操作数,插入数值
// 1.头插法 2.尾插法 3.插入中间位置
bool addNode(struct ListNode *p, int op, int number)
{
  // 考虑此时为空链表
  if(p == NULL)
  {
    return false;
  }
  // 头插法
  if(op == 1)
  {
    struct ListNode *s = (struct ListNode *)malloc(sizeof(struct ListNode));
    s->num = number;
    s->next = p->next;
    p->next = s;
  }
  // 尾插法
  if(op == 2)
  {
    struct ListNode *s = (struct ListNode *)malloc(sizeof(struct ListNode));
    s->next = p->next;
    while (s->next != NULL)
    {
      s = s->next;
    }
    struct ListNode *a = (struct ListNode *)malloc(sizeof(struct ListNode));
    s->next = a;
    a->num = number;
    a->next = NULL;
  }
  // 插入到中间位置
  if(op == 3)
  {
    struct ListNode *s = p;
    int position = 0; // 插入位置
    printf("请输入要插入的位置,只可以插入1-%d:\n", getListLength(p)-1);
    scanf("%d", &position);
    // 考虑边界
    if(position < 1 || position >= getListLength(p))
    {
      return false;
    }
    for (int i = 1; i <= position; i++)
    {
      s = s->next;
    }
    struct ListNode *a = (struct ListNode *)malloc(sizeof(struct ListNode));
    a->next = s->next;
    a->num = number;
    s->next = a;
  }
  return true;
}

// 删除元素 函数传入的是头指针,操作数
// 1.删除头部元素 2.删除尾部元素 3.删除中间元素
bool deleteNode(struct ListNode *p, int op)
{
  // 考虑空链表
  if(p == NULL)
  {
    return false;
  }
  // 删除头部元素
  if(op == 1)
  {
    p->next = p->next->next;
  }
  // 删除尾部元素
  if(op == 2)
  {
    struct ListNode *s = p;
    while(s->next->next != NULL)
    {
      s = s->next;
    }
    s->next = NULL;
  }
  if(op == 3)
  {
    struct ListNode *s = p;
    int position = 0;
    printf("请输入要删除结点的位置,当前链表总长度为%d:\n", getListLength(p));
    scanf("%d", &position);
    // 考虑边界情况
    if(position < 1 || position > getListLength(p))
    {
      return false;
    }
    for(int i = 1; i < position; i++)
    {
      s = s->next;
    }
    s->next = s->next->next;
  }
  return true;
}

// 输出链表
void printList(struct ListNode *p)
{
  struct ListNode *s = p;
  printf("链表长度为:%d\n链表数据为:\n", getListLength(p));
  while (s->next != NULL)
  {
    s = s->next;
    printf("%d ", s->num);
  }
  printf("\n");
}
int main()
{
  // p为头结点
  struct ListNode *p = NULL;
  p = initList();

  // 增
  int number = 0, op = 0;
  printf("输入操作数,1为表头插入,2为表尾插入,3为表内插入:\n");
  scanf("%d", &op);
  printf("输入插入的数据:\n");
  scanf("%d", &number);
  if(addNode(p, op, number))
  {
    printf("插入成功\n");
  }
  else
  {
    printf("插入失败\n");
  }
  printList(p);

  // 删
  printf("输入操作数,1为表头删除,2为表尾删除,3为表内删除:\n");
  scanf("%d", &op);
  if(deleteNode(p, op))
  {
    printf("删除成功\n");
  }
  else
  {
    printf("删除失败\n");
  }
  printList(p);

  return 0;
}
```
{% endfolding %}

---

{% image https://cdn.sakurasep.site/Website/About_Me.png 欢迎关注我的其他平台 %}