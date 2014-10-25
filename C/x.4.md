#冒泡排序算法
冒泡排序算法，例如我们有N个球，在一个天平上，有A,B两个球，轻的升上去，重的沉下去，然后再把重的球与下一个球进行比较，直到所有的球比对结束，最后我们就得到了最重的那个球           
然后我再从第一个球开始比较，比较到N-1球的时候就可以结束了，以后第一次我们已经得到了最重的球，我们现在得到的是第二重的球       
然后依次循环最后就得到了从轻到重的所有球          

我们有N个球，需要按从轻到重进行排序             
我们从第一个球开始，与下一个球进行比较，轻的放前面，重的放后面，再把重球与下一个球进行比较，轻的放前面，重的放后面，依次循环，直接最后一个，那么我们就把最重的球放到了最后面         
第二次，我们还是从第一个球开始进行比较，轻的放前面，重的放后面，但是这一次，我们只需要进行到N - 1次就可以了，为什么呢，因为我们在上一次已经得到了最重的球，再比较下去就是浪费时间            
不断的重复第二次，每一次都N = N -1 ,最后N = 0，也就结束了,我们就得到了，从轻到重的排序               

由此我们得出，要进行冒泡排序，首先需要一个数组和数组的长度，就可以进行排序了，因为是两两比对，所以比对的次数是长度减1            
如下的函数，我们就可以把一个整数数组进行从小到大的排序， 第一个参数是整数数组，第二个参数是数组的长度                    
```c
void xsort(int *a, int len)
{
    len -= 1;
    int *d = a;
    int *now = NULL;
    int *next = NULL;
    int i, j;
    for (i = 0; i < len; i++) {
        for (j = 0; j < len -i; j++) {
            now = d + j;
            next = d + j + 1;
            if (*now > *next) {
                *now  ^= *next;
                *next ^= *now;
                *now  ^= *next;
            }
        }
    }
}
```
上面是对整数进行排序，下面对字符串进行字符排序
```c
void strsort(char *src, int len)
{
    len -= 1;
    char* s = src;
    char* now = NULL;
    char* next = NULL;
    int i, j;
    for (i = 0; i < len; i++) {
        for (j = 0; j < len - i; j++) {
            now = s + j;
            next = s + j + 1;
            if (*now > *next) {
                *now  ^= *next;
                *next ^= *now;
                *now  ^= *next;
            }
        }
    }
}
```
示例
```c
#include <stdio.h>
#include <string.h>

void xsort(int *a, int len);

int main(int argc, char* argv[])
{
    int x[] = {10,20,4,5,1,8,30,100};
    int len = sizeof(x)/sizeof(x[0]);
    xsort(x, len);
    int i;
    
    for (i = 0; i < len; i++){
        printf("%d", x[i]);
        if (i != len - 1) printf(", ");
        else printf("\n");
    }
    
    return 0;
}

void xsort(int *a, int len)
{
    len -= 1;
    int *d = a;
    int *now = NULL;
    int *next = NULL;
    int i, j;
    for (i = 0; i < len; i++) {
        for (j = 0; j < len -i; j++) {
            now = d + j;
            next = d + j + 1;
            if (*now > *next) {
                *now  ^= *next;
                *next ^= *now;
                *now  ^= *next;
            }
        }
    }
}
```
示例2
```c
#include <stdio.h>
#include <string.h>

void strsort(char *src, int len);

int main(int argc, char* argv[])
{
    char str[1024];
    scanf("%s", str);
    strsort(str, strlen(str));
    printf("%s\n", str);
    
    return 0;
}

void strsort(char *src, int len)
{
    len -= 1;
    char* s = src;
    char* now = NULL;
    char* next = NULL;
    int i, j;
    for (i = 0; i < len; i++) {
        for (j = 0; j < len - i; j++) {
            now = s + j;
            next = s + j + 1;
            if (*now > *next) {
                *now  ^= *next;
                *next ^= *now;
                *now  ^= *next;
            }
        }
    }
}
```

