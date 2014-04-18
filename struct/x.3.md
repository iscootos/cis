#线性表
```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

#define MAXSIZE 20  /* 存储空间初始分配量 */
typedef int ElemType;
typedef struct {
	ElemType data[MAXSIZE];  /* 数组存储数据元素 */
	int length;  /* 线性表当前长度 */
} SqList;

#define OK 1
#define ERROR 0
#define TRUE 1
#define FALSE 0
typedef int Status;

/* 获取线性表的元素 */
Status GetElem(SqList L, int i, ElemType *e)
{
	if (L.length == 0 || i < 1 || i > L.length)
		return ERROR;
	*e = L.data[i - 1];
	return OK;
}

/* 插入线性表 */
Status ListInsert(SqList *L, int i, ElemType e)
{
	int k;
	if (L->length == MAXSIZE)
		return ERROR;
	if (i < 1 || i > L->length + 1)
		return ERROR;
	if (i <= L->length)
	{
		for (k = L->length - 1; k >= i - 1; k--)
			L->data[k + 1] = L->data[k];
	}
	L->data[i - 1] = e;
	L->length++;
	return OK;
}

/* 删除线性表 */
Status ListDelete(SqList *L, int i, ElemType *e)
{
	int k;
	if (L->length == 0)
		return ERROR;
	if (i < 1 || i > L->length)
		return ERROR;
	*e = L->data[i - 1];
	if (i < L->length)
	{
		for(k = i; k < L->length; k++)
			L->data[k - 1] = L->data[k];
	}
	L->length--;
	return OK;
}

int main(int argc, char *argv[])
{
	SqList sl;
	int sum = 10;
	int i;
	int x[] = {10,15,35,60,50,40,50,21,56,78,90};
	for(i = 1; i <= sum; i++)
	{
		sl.data[i - 1] = x[i - 1];
		
	}
	sl.length = sum;

	int a;
	GetElem(sl, 6, &a);
	printf("%d\n", a);
}
```
