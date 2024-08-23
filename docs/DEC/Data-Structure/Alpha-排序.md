# Alpha 排序



## 直接插入排序算法的实现

请编写程序，完成`InsertSort`函数，给定一个整数数组 a 和数组的长度 length，实现直接插入排序算法。

### 示例输出

```
13 27 38 49 51 65 76 97
```

#### 题目

```c
//直接插入排序 
#include<stdio.h>

//请完成InsertSort函数，实现"直接插入排序"的功能 
//参数：L，排序的数列
//length, 排序的数列的长度
void InsertSort(int *L, int length)
{
	//请在此处填写代码 
    
	
}

int main(){
    int a[]={49,38,65,97,76,13,27,51};
    int n = sizeof(a) / sizeof(a[0]);
	InsertSort(a,n);
	int i; 
    for(i = 0; i < n; i++)
        printf("%d ",a[i]);
    return 0;
}

```

### 标准答案

```c
void InsertSort(int* L, int length)
{
    int i, j;
    for (i = 1; i < length; i++) {
        if (L[i] < L[i - 1]) {
            int temp = L[i];
            for (j = i - 1; j >= 0 && L[j] > temp; j--) {
                L[j + 1] = L[j];
            }
            L[j + 1] = temp;
        }
    }
}
```





## 单链表的简单选择排序（待解决）

设计一个程序，实现对单链表的简单选择排序。用户输入链表的元素个数和数据，程序创建一个单链表，并对其进行升序排序。最后，程序输出排序后的链表。

### 示例

**输入**

```
链表的元素个数为：4

请输入链表的数据：
单链表中第 1个元素是：3
单链表中第 2个元素是：-1
单链表中第 3个元素是：1
单链表中第 4个元素是：2
```

**输出**

```
排序后的链表为：-1 1 2 3
```

#### 题目

```c
    #include <stdio.h>
    #include <stdlib.h>

    typedef struct LNode {
        int data;
        struct LNode *next;
    } LNode, *LinkList;

    void Create_LinkList(LinkList *L, int n) {
        //创建链表
        //请在此处编写代码


    }

    void print_LinkList(LinkList L) {
        //输出链表
        //请在此处编写代码


    }

    void SelectSort(LinkList *L) {
        //简单选择排序
        //请在此处编写代码


    }

    int main() {
        //创建链表
        LinkList L;

        //输入数据
        int n;
        printf("链表的元素个数为：");
        scanf("%d", &n);
        printf("\n请输入链表的数据：\n");
        Create_LinkList(&L, n);

        SelectSort(&L);
        printf("\n排序后的链表为：");
        print_LinkList(L);

        // 释放链表内存
        LinkList p = L, temp;
        while (p) {
            temp = p->next;
            free(p);
            p = temp;
        }

        return 0;
    }

```

### 标准答案

```c
void Create_LinkList(LinkList* L, int n) {
    *L = (LinkList)malloc(sizeof(LNode)); //创建头结点
    (*L)->next = NULL;

    LinkList rear; //尾指针
    rear = *L;     //指向头结点

    for (int i = 0; i < n; i++) {
        LinkList temp = (LinkList)malloc(sizeof(LNode)); //给新结点创建空间
        temp->next = NULL;
        printf("单链表中第%2d个元素是：", i + 1); //下标是从0开始的，得加1
        scanf("%d", &temp->data);
        rear->next = temp; //新结点插入链表尾部
        rear = temp;       //rear指向新的尾节点
    }
}

void print_LinkList(LinkList L) {
    LinkList p = L;
    while (p->next != NULL) {
        p = p->next; //跳过头结点输出下一个结点中存储的数值
        printf("%d ", p->data);
    }
}

void SelectSort(LinkList* L) {
    //简单选择排序
    LinkList left, right, p = (*L)->next;
    int temp;

    while (p) {
        right = p->next;
        left = p;

        while (right) {
            if (right->data < left->data) {
                left = right;
            } 
            right = right->next;
        }

        if (left != p) {
            temp = left->data;
            left->data = p->data;
            p->data = temp;
        }
        p = p->next;
    }
}
```





## 冒泡排序的实现

请编写程序，完成`BubbleSort`函数，给定一个整数数组 a 和数组的长度 n，实现冒泡排序算法，输出每趟排序后的数组。

### 示例输出

```
38 49 65 76 13 27 51 97 
38 49 65 13 27 51 76 97 
38 49 13 27 51 65 76 97 
38 13 27 49 51 65 76 97 
13 27 38 49 51 65 76 97
```

![bubbleSort](./Alpha-%E6%8E%92%E5%BA%8F.assets/bubbleSort.gif)

#### 题目

```c
//冒泡排序 
#include<stdio.h>

//请完成BubbleSort函数，实现"冒泡排序"的功能 
//参数：a，排序的数列
//参数：n, 排序的数列的长度
void BubbleSort(int *a, int n){
    //请在此处编写代码
    
    
}

int main(){
    int a[] = {49, 38, 65, 97, 76, 13, 27, 51};
    int n = sizeof(a) / sizeof(a[0]);
    BubbleSort(a, n);
    return 0;
}
```

### 标准答案

```c
void BubbleSort(int* a, int n) {
    int i, j;
    int change;

    for (i = 0; i < n - 1; i++) {
        change = 0;
        for (j = 0; j < n - i - 1; j++) {
            if (a[j] > a[j + 1]) {
                int temp = a[j];
                a[j] = a[j + 1];
                a[j + 1] = temp;
                change = 1;
            }
        }

        if (change = 0) {
            break;
        }

        for (int k = 0; k < n; k++) {
            printf("%d ", a[k]);
        }
        printf("\n");
    }
}
```





## 实现冒泡排序算法

编写一个程序，实现冒泡排序算法，对给定的整数数组进行升序排序。完善函数 Bubble_Sort，该函数接受一个整数数组和数组长度作为参数，并在原地对数组进行排序。排序完成后，输出排序后的数组。

### 示例

**输入**

```
在主函数中设置str[] = {3,1,7,8,10,6,5,9,12,0};
```

**输出**

```
冒泡排序后的序列为：0 1 3 5 6 7 8 9 10 12
```

#### 题目

```c
#include <stdio.h>

void Bubble_Sort(int *str,int n){
     //冒泡排序
    //请在此处编写代码
    
	
}

int main(){	 
    //根据题意，完成程序
    
     int str[] = {2,5,6,3,7,8,0,9,12,1};  
     Bubble_Sort(str, 10);
     printf("冒泡排序后的序列为："); 
     for (int i = 0; i < 10; i++)
          printf("%d ",str[i]);
     return 0;
}
```

### 标准答案

```c
void Bubble_Sort(int* str, int n) {
    int temp, i, j;

    for (i = 0; i < n - 1; i++) {
        for (j = 0; j < n - 1 - i; j++) {
            if (str[j] > str[j + 1]) {
                temp = str[j];
                str[j] = str[j + 1];
                str[j + 1] = temp;
            }
        }
    }
}
```





## 快速排序的实现

请编写程序，完成`QuickSort`函数，给定一个整数数组 a 和数组的长度 n，实现快速排序算法，并输出排序后的数组。

### 示例输出

```
13 27 38 49 51 65 76 97
```

![quickSort](./Alpha-%E6%8E%92%E5%BA%8F.assets/quickSort.gif)

#### 题目

```c
#include<stdio.h>
 
//请完成QuickSort函数，实现快速排序 
void QuickSort(int a[], int start_num,int end_num)
{ 
	//请在此处填写代码 

}

int main(){
	int a[]={49,38,65,97,76,13,27,51};
    int n=sizeof(a)/sizeof(a[0]);
	QuickSort(a,0,n-1);
	int i;
	for(i = 0;i < sizeof(a)/sizeof(int);i++)
		printf("%d ",a[i]);
	return 0;
} 
```

#### 答案

```C
// 交换数组中两个元素的值
void swap(int* a, int* b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

// 快速排序
void QuickSort(int a[], int start_num, int end_num) {
    if (start_num >= end_num) {
        // 当子数组只有一个元素时，已经有序，无需排序
        return;
    }

    int pivot = a[start_num]; // 选择第一个元素作为基准值
    int left = start_num + 1; // 左指针从基准值的下一个元素开始
    int right = end_num;      // 右指针从数组末尾开始

    while (left <= right) {
        while (left <= right && a[left] <= pivot) // 在左侧找到第一个大于基准值的元素
            left++;
        while (left <= right && a[right] >= pivot) // 在右侧找到第一个小于基准值的元素
            right--;

        if (left < right) // 如果左右指针没有相遇，则交换两个元素
            swap(&a[left], &a[right]);
    }

    swap(&a[start_num], &a[right]); // 将基准值放到正确的位置上

    QuickSort(a, start_num, right - 1); // 递归排序左侧子数组
    QuickSort(a, right + 1, end_num);   // 递归排序右侧子数组
}
```

### 标准答案（不太好）

```c
void QuickSort(int a[], int start_num, int end_num)
{
    int i, j, tmp;

    if (start_num < end_num)
    {
        i = start_num;
        j = end_num + 1;
        while (1)
        {
            do {
                i++;
            } while (!(a[start_num] <= a[i] || i == end_num));

            do {
                j--;
            } while (!(a[start_num] >= a[j] || j == start_num));

            if (i < j) {
                tmp = a[i];
                a[i] = a[j];
                a[j] = tmp;
            }
            else {
                break;
            }
        }

        tmp = a[start_num];
        a[start_num] = a[j];
        a[j] = tmp;

        QuickSort(a, start_num, j - 1);
        QuickSort(a, j + 1, end_num);
    }
}
```





## 堆排序的实现

请编写程序，完成`HeapAdjust`函数，给定一个整数数组 a 和数组的长度 n，实现堆排序算法，并输出每次堆排序的结果。

### 示例输出

```
49 38 65 97 76 13 27 51 
49 38 65 97 76 13 27 51 
49 97 65 51 76 13 27 38 
97 76 65 51 49 13 27 38 
76 51 65 38 49 13 27 97 
65 51 27 38 49 13 76 97 
51 49 27 38 13 65 76 97 
49 38 27 13 51 65 76 97 
38 13 27 49 51 65 76 97 
27 13 38 49 51 65 76 97 
13 27 38 49 51 65 76 97
```

![heapSort](./Alpha-%E6%8E%92%E5%BA%8F.assets/heapSort.gif)

#### 题目

```c
#include <stdio.h>

//请完成HeapAdjust函数，实现堆排序一次筛选的过程
//已知a[s..m]中除a[s]之外均满足堆的定义，通过本函数调整，使a[s..m]成为一个大顶堆 
void HeapAdjust(int a[],int s,int m)
{
	//请在此次完成代码
    
    

}

// 对顺序表a进行堆排序 
// n为顺序表的长度 
void HeapSort(int a[], int n)
{
    int temp, i, j;

    for (i = (n /2) - 1; i >= 0; i--) // 构建大顶堆
        // 注：数组下标从0开始，所以i应该从n/2-1开始，因为数组长度为n，所以最后一个非叶子节点的下标为n/2-1
    {
        HeapAdjust(a, i, n - 1);

        for (j = 0; j < 8; j++) {
            printf("%d ", a[j]);  // 输出堆排序过程
        }
        printf("\n");
    }

    printf("\n");

    for (i = n - 1; i > 0; i--)
    {
        // 每次将堆顶元素与最后一个元素交换，然后调整堆，使其成为大顶堆
        temp = a[0];
        a[0] = a[i];
        a[i] = temp;

        HeapAdjust(a, 0, i - 1);  // 调整堆产生新的大顶堆

        for (j = 0; j < 8; j++) {
            printf("%d ", a[j]);
        }  // 输出排序结果
        printf("\n");
    }
}

int main()
{
    int a[]={49,38,65,97,76,13,27,51};
    int n=sizeof(a)/sizeof(a[0]);
    HeapSort(a,n);
}

```

### 标准答案

```c
// 请完成HeapAdjust函数，实现堆排序一次筛选的过程
// 已知 a[s..m] 中除 a[s] 之外均满足堆的定义，通过本函数调整，使 a[s..m] 成为一个大顶堆 
void HeapAdjust(int a[], int s, int m)
{
    int rc, j; // rc为父节点，j为子节点
    rc = a[s]; // 父节点

    for (j = 2 * s + 1; j <= m; j = j * 2 + 1) // 注：数组下标从0开始，所以两个子节点是2s+1,2s+2 
        // 也就是j从s的左子节点开始，一直到m，每次循环之后将 j 指向自己的左子
    { 
        if (j < m && a[j] < a[j + 1]) { // 比较左右子节点，找出较大的子节点
            j++; // 若右子节点大于左，则选取右子节点
        }

        if (rc > a[j]) { // 若父节点大于子节点，则不需要再交换，直接退出循环
            break;
        }

        a[s] = a[j]; // 父节点与较大的子节点交换
        s = j; // 继续调整子节点
    }
    a[s] = rc; // 父节点与其子节点交换完后，将父节点放到正确位置
}
```





## 实现堆排序算法

编写一个程序，实现堆排序算法。程序获取一个整数数组，以及数组的元素个数。完善函数 Creat_Heap 和 Heap_Sort 使用堆排序算法对输入的整数数组进行升序排序。最后输出排序后的整数数组。

### 示例

**输入**

```
请输入数组的元素个数：4
请输入数组元素：56 23 76 44
```

**输出**

```
堆排序后的结果为：23 44 56 76
```

#### 题目

```c
#include<stdio.h>
#define MAX 100

void Insert_Array(int array[],int n){
     //输入数组 
    int i;
    for(i=0;i<n;i++){
        scanf("%d",&array[i]);//输入数据用空格分隔开 
    }
}

void Print_Array(int array[],int n){
     //输出数组
    int i;
    for(i=0;i<n;i++){
        printf("%d ",array[i]);
    }
}

void Creat_Heap(int  array[], int i, int n){
     //构造堆，调整元素位置 
	 
}

void Heap_Sort(int array[], int n){
     //堆排序 
	
}

int main(){
     //根据题意，完成程序 
	
     int n,array[MAX];
     printf("请输入数组的元素个数：");
     scanf("%d",&n); 
	
     printf("请输入数组元素：");
     Insert_Array(array,n);
	
     printf("\n堆排序后的结果为：");
     Heap_Sort(array,n);
     Print_Array(array,n);
     return 0;
}
```

### 标准答案

> 对于这个题，我的建议就是好好看看上一个题，建议去`bilibili`看看视频

```C
void Creat_Heap(int  array[], int i, int n) {
    int lchild, temp = array[i];

    for (; 2 * i + 1 < n; i = lchild) {
        lchild = 2 * i + 1; //求左孩子

        if (lchild != n - 1 && array[lchild + 1] > array[lchild]) {
            lchild = lchild + 1; //如果lchild不是最后一个元素，比较左右孩子，找最大 
        }
        if (temp < array[lchild]) {
            array[i] = array[lchild];
        }
        else {
            break;
        }
    }
    array[i] = temp;
}

void Heap_Sort(int array[], int n) {
    //堆排序 
    int i, temp;

    for (i = n / 2; i >= 0; --i) //i表示比较位置，第一个位置 i = n / 2
        Creat_Heap(array, i, n);
    for (i = n - 1; i > 0; --i) {
        temp = array[0];
        array[0] = array[i];
        array[i] = temp; //将最大元素（堆顶）与最后一个元素交换
        
        Creat_Heap(array, 0, i); //删除最大元素，重新构造堆 
    }
}
```





## 2-路归并排序算法的实现

请编写程序，完成`Merge`函数，给定一个整数数组 a 和数组的长度 n，实现`2-路归并排序`算法，并输出每次归并排序的结果。

### 示例输出

```
38 49 
38 49 65 97 
38 49 65 97 
38 49 65 97 13 76 
38 49 65 97 13 76 27 51 
38 49 65 97 13 27 51 76 
13 27 38 49 51 65 76 97 
```

![mergeSort](./Alpha-%E6%8E%92%E5%BA%8F.assets/mergeSort.gif)

#### 题目

```c
#include<stdio.h>

//请完成Merge函数，实现2-路归并排序功能 
//该函数可将有序的SR[i..m]和SR[m+1..n]归并为有序的TR[i..n]
void Merge(int SR[],int TR[],int i,int m,int n){ 
	//请在此完成代码 
    
}

//将SR[s..t] 归并排序为TR1[s..t] 
void MSort(int SR[], int TR1[], int s, int t)
{
	int TR2[8];
    if(s == t) 
		TR1[s] = SR[s];
	else 
    {
        int m = (s+t)/2 ;
        MSort(SR,TR2,s,m) ;
        MSort(SR,TR2,m+1,t) ;
        Merge(TR2,TR1,s,m,t) ;
        int i;
        for(i = 0; i <=t ;i++)
	    	printf("%d ",TR1[i]);
	    printf("\n");
    }
}

int main()
{
    int a[]={49,38,65,97,76,13,27,51};
    int n=sizeof(a)/sizeof(a[0]);
    int b[8];
    MSort(a,b,0,n-1);
    return 0 ;
}

```

### 标准答案

> 咱要完成的部分并不难，就是合并

```c
//请完成Merge函数，实现2-路归并排序功能 
//该函数可将有序的 SR[i..m] 和 SR[m+1..n] 归并为有序的 TR[i..n]
void Merge(int SR[], int TR[], int i, int m, int n) {
    //请在此完成代码 
    int j = m + 1, k = i;

    // 将SR[i..m] 和 SR[j..n] 归并为TR[i..n]
    while ((i <= m) && (j <= n))
    {
        if (SR[i] < SR[j]) {
            TR[k++] = SR[i];
            i = i + 1;
        }
        if (SR[i] > SR[j]) {
            TR[k++] = SR[j];
            j = j + 1;
        }
    }

    while (i <= m) {
        TR[k++] = SR[i++];
    }
    while (j <= n) {
        TR[k++] = SR[j++];
    }
}
```





## 编写希尔排序算法

编写一个程序，实现希尔排序算法。程序获取一个整数数组，以及数组的元素个数。完善函数 Shell_Sort 使用希尔排序算法对输入的整数数组进行升序排序。最后输出排序后的整数数组。

### 示例

**要求**

```
1、增量用increment表示
2、增量的取法：increment=increment/3向下取整+1
```

**输入**

```
在主函数中设置str[] = {3,1,7,8,10,6,5,9,12,0};
```

**输出**

```
希尔排序后的序列为：0 1 3 5 6 7 8 9 10 12
```

![Sorting_shellsort_anim](./Alpha-%E6%8E%92%E5%BA%8F.assets/Sorting_shellsort_anim.gif)

#### 题目

```c
#include <stdio.h>
 
void Shell_Sort(int *str,int count){
     //希尔排序 
	
}

int main(){	 
    //根据题意，完成程序
    
     int str[] = {3,1,7,8,10,6,5,9,12,0};
     Shell_Sort(str, 10);
     printf("希尔排序后的序列为："); 
     for (int i = 0; i < 10; i++)
          printf("%d ",str[i]);
     return 0;
}
```

#### 答案

```C
void Shell_Sort(int* str, int count) {
    // 希尔排序
    int increment = count / 3 + 1; // 初始增量

    while (increment > 1) {
        for (int i = 0; i < count - increment; i++) {
            if (str[i] > str[i + increment]) { // 交换
                int temp = str[i];
                str[i] = str[i + increment];
                str[i + increment] = temp;
            }
        }
        increment = increment / 3 + 1; // 更新增量
    }

    // 最后一趟插入排序
    for (int i = 1; i < count; i++) {
        int temp = str[i];
        int j = i - 1;

        while (j >= 0 && str[j] > temp) {
            str[j + 1] = str[j];
            j--;
        }
        str[j + 1] = temp;
    }
}
```

**最后一趟的插入排序：**

![insertionSort](./Alpha-%E6%8E%92%E5%BA%8F.assets/insertionSort.gif)

### 标准答案（不太好）

> 怎么说呢，上边那个答案更…规范…一些，就是更符合希尔排序的标准流程。

```c
void Shell_Sort(int *str,int count){
	//希尔排序 
	int increment = count;//设置增量初始值 
 	while(increment > 1){
    	 increment = increment / 3 + 1;
    	 for(int i = increment; i < count; i++){
          int key = str[i];
          int j = i - increment;
   			while(j >= 0 && str[j] > key){
          str[j+increment] = str[j];
     			  j = j - increment;
   			}
    		str[j+increment] = key;
  		}
 	}
}
```

