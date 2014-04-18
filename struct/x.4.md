#单链表
```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

#define OK 1
#define ERROR 0
typedef ElemType int;

/* 线性表的单链表存储结构 */
typedef struct Node{
	ElemType data;
	struct Node *next;
} Node;
typedef struct Node *LinkList;  /* 定义LinkList */


typedef Status int;

Status GetElem(LinkList L, int i, ElemType *e)
{
	int j;
	LinkList p;  /* 声明一个节点p */
	p = L->next;  /* 让p指向链表L的第一个节点 */
	j = l;  /* j为计数器 */
	while(p && j < i)  /* p不为空或者计数器j还没有等于i时，循环继续 */
	{
		p = p->next;  /* 让p指向下一个节点 */
		++j;
	}
	if (!p || j > i)
		return ERROR;  /* 第i个元素不存在 */
	*e = p->data;  /* 获取第i个元素的数据 */
	return OK;
}

Status ListInsert(LinkList *L, int i, ElemType e)
{
	int j;
	LinkList p,s;
	p = *L;
	j = 1;
	while(p && j < i)  /* 寻找第i个节点 */
	{
		p = p->next;
		++j;
	}
	if (!p || j > i)
		return ERROR;  /* 第i个元素不存在 */

	s = (LinkList)malloc(sizeof(Node));  /* 生成新节点 */
	s->data = e;
	s->next = p->next;  /* 将p的后继节点赋值给s的后继 */
	p->next = s;  /* 将s赋值给p的后继 */
	return OK;
}

Status ListDelete(LinkList *L, int i, ElemType *e)
{
	int j;
	LinkList p, q;
	p = *L;
	j = 1;
	while (p->next && j < i)  /* 遍历寻找第i个元素 */
	{
		p = p->next;
		++j;
	}
	if (!(p->next) || j > i)
		return ERROR;  /* 第i个元素不存在 */
	q = p->next;
	p->next = q->next;  /* 将q后继赋值给p的后继 */
	*e = q->data;  /* 将q节点中的数据给e */
	free(q);  /* 回收节点，释放内存 */
	return OK;
}

void CreateListHead(LinkList *L, int n)
{
	LinkList p;
	int i;
	srand(time(0));  /* 初始化随机数种子 */
	*L = (LinkList)malloc(sizeof(Node));
	(*L)->next = NULL;  /* 先建立一个带头节点的单链表 */
	for (i = 0; i < n; i++)
	{
		p = (LinkList)malloc(sizeof(Node));  /* 生成新节点 */
		p->data = rand()%100 + 1;  /* 随机生成100以内的数字 */
		p->next = (*L)->next;
		(*L)->next = p;  /* 插入到表头 */
	}
}

void CreateListTail(LinkList *L, int n)
{
	LinkList p,r;
	int i;
	srand(time(0));  /* 初始化随机数种子 */
	*L = (LinkList)malloc(sizeof(Node));  /* 整个线性表 */
	r = *L; /* 指向尾部的节点 */
	for (i = 0; i < n; i++)
	{
		p = (Node *)malloc(sizeof(Node));  /* 生成新节点 */
		p->data = rand()%100 + 1;  /* 随机生成100以内的数字 */
		r->next = p;  /* 将表尾终端节点的指针指向新节点 */
		r = p;  /* 将当前新节点定义为表尾终端节点 */
	}
	r->next = NULL;  /* 当前链表结束 */
}
```


