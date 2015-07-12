#快速排序算法
快速排序是对冒泡排序的一种改进，把一个问题分隔成若干个子问题进行解决后再组合起来的思想          
假设要排序的数组A[1]...A[N]，首先任意选取一个数据,通常选用第一个数据作为关键数据，然后从右往左进行一次遍历，把比它小的放到前面，把比它大的放到后面，这个过程叫做一趟快速排序，一趟快速排序的算法是       
设置两个变量i,j排序开始的时候i = 1, j = N        
以第一个数组元素作为关键数据，赋值给X, X = A[1]         
从j开始向前搜索，即由后向前开始搜索j = j - 1，找到第一个小于X的值，两者交换         
从i开始向后搜索，即由前开始向后搜索，i = i + 1,找到第一个大于X的值，两者交换        
重复以上两步，知道i == j        


```c
/*
 * C.A.R.Hoare 快速排序
 */
void hoare_quicksort(int *array, int left, int right)
{
	int i, j, t;

	if (left >= right)
		return;

	t = array[left];  //基准数
	i = left;
	j = right;

	while (i < j) {
		//顺序很重要，要先从右往左找
		while (array[j] >= t && i < j)
			j--;
		//再从左往右找
		while (array[i] <= t && i < j)
			i++;
		//交换两个数在数组中的位置
		if (i < j) {  //当哨兵i和哨兵j没有相遇时
			array[i] ^= array[j];
			array[j] ^= array[i];
			array[i] ^= array[j];
		}
	}

	array[left] = array[i];
	array[i] = t;

	hoare_quicksort(array, left, i - 1);  //继续处理左边的，这里是一个递归的过程
	hoare_quicksort(array, i + 1, right);  //继续处理右边的，这里是一个递归的过程
}
```
```c
/*
 * N.Lomuto 快速排序
 */
void lomuto_quicksort(int *array, int left, int right)
{
	int i, j, t;

	if (left >= right)
		return;

	t = array[right];  //基准数
	i = left;

	//哨兵j先从左往右走
	for (j = left; j < right; j++) {
		//直到遇到小于等于基准数的位置停下，交换哨兵i和哨兵j的值，然后哨兵i走到下一步
		if (array[j] <= t && i < j) {
			array[i] ^= array[j];
			array[j] ^= array[i];
			array[i] ^= array[j];
			i++;
		}
	}
	
	if (array[i] > t) {
		array[right] = array[i];
		array[i] = t;
		lomuto_quicksort(array, left, i - 1);
		lomuto_quicksort(array, i + 1, right);
	} else {
		lomuto_quicksort(array, left, i);
	}
}
```