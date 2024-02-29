# 顺序表SeqList详解


## 线性表

*   线性表（linear list）是 n 个具有相同特性的数据元素的有限序列。 线性表是一种在实际中广泛使用的数据结构，常见的线性表：顺序表、链表、栈、队列、字符串…
    
*   线性表在逻辑上是线性结构，也就说是连续的一条直线。但是在物理结构上并不一定是连续的，线性表在物理上存储时，通常以数组和链式结构的形式存储。
    



## 顺序表

### 1 什么是顺序表

*   顺序表是用一段**物理地址连续的存储单元**依次存储数据元素的线性结构，一般情况下采用数组存储。在数组 上完成数据的增删查改。
    
*   顺序表：**可动态增长的数组，要求数据是连续存储的**
    



### 2 顺序表的定义

1、静态顺序表：使用定长数组存储元素

*   缺陷：给小了不够用，给大了可能浪费，非常不实用

```c
#define N 10
typedef int SLDataType;

typedef struct SeqList
{
	SLDataType array[N];  //定长数组
	size_t size;          //有效数据个数
}SeqList;
```

2、**动态顺序表**：使用动态开辟的数组存储元素

*   动态顺序表可根据我们的需要分配空间大小
*   size 表示当前顺序表中已存放的数据个数
*   capacity 表示顺序表总共能够存放的数据个数

```c
typedef int SLDataType; //类型重命名，后续要存储其它类型时方便更改

typedef struct SeqList
{
	SLDataType* a;    //指向动态开辟的数组
	size_t size;      //有效数据个数
	size_t capacity;  //容量大小
}SeqList;
```



### 3 顺序表的接口实现

首先新建一个工程此次用的是动态顺序表

*   SeqList.h（顺序表的类型定义、接口函数声明、引用的头文件）
*   SeqList.c（顺序表接口函数的实现）
*   Test.c（主函数、测试顺序表各个接口功能）



#### 头文件

*   SeqList.h 头文件代码如下：

```c
#define _CRT_SECURE_NO_WARNINGS 1
#pragma once  //防止头文件被二次引用

#include <stdio.h>   /*perror, printf*/
#include <assert.h>  /*assert*/
#include <stdlib.h>  /*realloc*/

//后续要存储其它类型时方便更改
typedef int SLDataType;  

//顺序表的动态存储
typedef struct SeqList
{
	SLDataType* a;    //指向动态开辟的数组
	int size;      //有效数据个数
	int capacity;  //容量大小
}SeqList;

//初始化顺序表
void SeqListInit(SeqList* psl);

//销毁顺序表
void SeqListDestory(SeqList* psl);

//检查顺序表容量是否满了，好进行增容
void CheckCapacity(SeqList* psl);

//顺序表尾插
void SeqListPushBack(SeqList* psl, SLDataType x);//O(1)

//顺序表尾删
void SeqListPopBack(SeqList* psl);//O(1)

//顺序表头插
void SeqListPushFront(SeqList* psl, SLDataType x);//O(n)

//顺序表头删
void SeqListPopFront(SeqList* psl);//O(n)

//打印顺序表
void SeqListPrint(const SeqList* psl);

//在顺序表中查找指定值
int SeqListFind(const SeqList* psl, SLDataType x);

//在顺序表指定下标位置插入数据
void SeqListInsert(SeqList* psl, size_t pos, SLDataType x);

//在顺序表中删除指定下标位置的数据
void SeqListErase(SeqList* psl, size_t pos);

//查看顺序表中数据个数
int SeqListSize(const SeqList* psl);

//修改指定下标位置的数据
void SeqListAt(SeqList* psl, size_t pos, SLDataType x);

```



###  SeqList.c 中各个接口函数

#### 1、初始化顺序表

记得一定要加上断言，防止传进来的指针为空

```c
void SeqListInit(SeqList* psl)
{
	assert(psl != NULL);  //断言

	psl->a = NULL;  //初始顺序表为空
	psl->size = 0;  //初始数据个数为0
	psl->capacity = 0;  //初始空间容量为0
}
```

#### 2、销毁（释放）顺序表

记得一定要加上断言，防止传进来的指针为空

```c
void SeqListDestory(SeqList* psl)
{
	assert(psl != NULL);  //断言

	free(psl->a);   //释放动态开辟的空间
	psl->a = NULL;  //置空
	psl->size = 0;  //数据个数置0
	psl->capacity = 0;  //空间容量大小置0
}
```

#### 3、检查顺序表容量是否满了，好进行增容

为什么不采取插一个数据，增容一个空间的方式呢，因为这样也太麻烦了，代价也太大了，一般情况下，为了避免频繁的增容，当空间满了后，我们不会一个一个的去增，而是一次增容 2 倍，当然也不会一次增容太大，比如 3 倍 4 倍，空间可能会浪费，2 倍是一个折中的选择。

```c
void CheckCapacity(SeqList* psl)
{
	assert(psl != NULL);  //断言

	if (psl->size == psl->capacity)  //检查容量，满了则增容
	{
		size_t newcapacity;  //新容量
		if (psl->capacity == 0)
			newcapacity = psl->capacity = 4;  //原来容量为0，扩容为4
		else
			newcapacity = 2 * psl->capacity;  //原来容量不为0，扩容为原来的2倍
		
		SLDataType* p = (SLDataType*)realloc(psl->a, newcapacity*sizeof(SLDataType));  //扩容
		if (p == NULL)
		{
			perror("realloc");
			exit(-1);
		}
		psl->a = p;  // p 不为空，开辟成功
		psl->capacity = newcapacity;  //更新容量
	}
}
```

#### 3、顺序表尾插

```c
void SeqListPushBack(SeqList* psl, SLDataType x)
{
	assert(psl != NULL);  //断言

	CheckCapacity(psl);  //检查顺序表容量是否已满

	psl->a[psl->size] = x;  //尾插数据
	psl->size++;  //有效数据个数+1
}
```

#### 4、顺序表尾删

不知道 SLDataType 是什么类型的数据，不能冒然的将顺序表最后一个数据赋值为 0，我们只需将有效数据个数 size 减 1 即可达到尾删的目的。

```c
void SeqListPopBack(SeqList* psl)
{
	assert(psl != NULL);  //断言
	assert(psl->size > 0);  //顺序表不能为空

	//不知道SLDataType是什么类型的数据，不能冒然的赋值为0
	//psl->a[psl->size - 1] = 0;
	psl->size--;  //有效数据个数-1
}
```

#### 5、顺序表头插

因为顺序表是连续存储的，所以头插时要依次挪动数据

```c
void SeqListPushFront(SeqList* psl, SLDataType x)
{
	assert(psl);  //断言
	CheckCapacity(psl);  //检查顺序表容量是否已满
    
	int i = 0;
	for (i = psl->size - 1; i >= 0; i--)  //顺序表中[0,size-1]的元素依次向后挪动一位
	{
		psl->a[i + 1] = psl->a[i];
	}
	psl->a[0] = x;  //头插数据
	psl->size++;  //有效数据个数+1
}
```

#### 6、顺序表头删

因为顺序表是连续存储的，所以头删时要依次挪动数据

```c
void SeqListPopFront(SeqList* psl)
{
	assert(psl);  //断言
	assert(psl->size > 0);  //顺序表不能为空

	int i = 0;
	for (i = 1; i < psl->size; i++)  //顺序表中[1,size-1]的元素依次向前挪动一位
	{
		psl->a[i - 1] = psl->a[i];
	}
	psl->size--;  //有效数据个数-1
}
```

#### 7、打印顺序表

```c
void SeqListPrint(const SeqList* psl)
{
	assert(psl != NULL);  //断言

	if (psl->size == 0)  //判断顺序表是否为空
	{
		printf("顺序表为空\n");
		return;
	}

	int i = 0;
	for (i = 0; i < psl->size; i++)  //打印顺序表
	{
		printf("%d ", psl->a[i]);
	}
	printf("\n");
}
```

#### 8、在顺序表中查找指定值

```c
int SeqListFind(const SeqList* psl, SLDataType x)
{
	assert(psl);  //断言

	int i = 0;
	for (i = 0; i < psl->size; i++)
	{
		if (psl->a[i] == x)
		{
			return i;  //查找到，返回该值在数组中的下标
		}
	}
	return -1;  //没有查找到
}
```

#### 9、在顺序表指定下标位置插入数据（要注意下 int 与 size_t 间的转换问题）

将指定位置后的所有数据依次向后挪动一位，初始代码如下：

```c
int i = 0;
for (i = psl->size - 1; i >= pos; i--)
	psl->a[i + 1] = psl->a[i];
```

*   原先这种写法，当顺序表为空 size = 0 时，会导致 i = -1，  
    执行 i >= pos 时，i 被算术转换成无符号数，而无符号数的 -1 是一个值很大的正数，  
    远大于 pos，满足条件进入循环，会造成越界访问
*   注：转换并不会改变 i 本身的值，而是执行 i >= pos 时，生成一个临时的值与 pos 比较
*   如果在顺序表头部插入数据 pos = 0，i 最终也会减减变成 -1，被算术转换后变成一个很大的数
*   总结：**避免负数给到无符号数**，或者**避免有符号数变成负数**后，被算术转换或整型提升后，变成一个很大的数

下面这样写就避免 i 变成 -1 负数了

```c
void SeqListInsert(SeqList* psl, size_t pos, SLDataType x)
{
	assert(psl);  //断言
	assert(pos >= 0 && pos <= psl->size);  //检查pos下标的合法性

	CheckCapacity(psl);  //检查顺序表容量是否已满

	size_t i = 0;
	for (i = psl->size; i > pos; i--)  //将pos位置后面的数据依次向后挪动一位
	{
		psl->a[i] = psl->a[i - 1];
	}
	psl->a[pos] = x;  //插入数据
	psl->size++;  //有效数据个数+1
}
```

*   实现了此接口，顺序表头插相当于在下标为 0 位置处插入数据，可以改进下顺序表头插的代码：

```C
void SeqListPushFront(SeqList* psl, SLDataType x)
{
	SeqListInsert(psl, 0, x);  //改造头插接口
}
```

#### 10、在顺序表中删除指定下标位置的数据

```C
void SeqListErase(SeqList* psl, size_t pos)
{
	assert(psl);  //断言
	assert(psl->size > 0);  //顺序表不能为空
	assert(pos >= 0 && pos < psl->size);  //检查pos下标的合法性

	size_t i = 0;
	for (i = pos + 1; i < psl->size; i++)  //将pos位置后面的数据依次向前挪动一位
	{
		psl->a[i - 1] = psl->a[i];
	}
	psl->size--;  //有效数据个数-1
}
```

*   实现了此接口，顺序表头删相当于删除下标为 0 位置处的数据，可以改进下顺序表头删的代码：

```C
//顺序表头删
void SeqListPopFront(SeqList* psl)
{
	SeqListErase(psl, 0);  //改造头删接口
}
```

#### 11、查看顺序表中有效数据个数

*   可能大家会有一个疑问，我在主函数里面直接通过定义的结构体变量直接访问就好了呀，为啥还要弄一个函数嘞
    
*   在数据结构中有一个约定，如果要访问或修改数据结构中的数据，不要直接去访问，要去调用它的函数来访问和修改，这样更加规范安全，也更方便检查是否出现了越界等一些错误情况
    

```c
size_t SeqListSize(const SeqList* psl)
{
	assert(psl);  //断言

	return psl->size;
}
```

> **越界不一定报错，系统对越界的检查是一种抽查**
> 
> *   越界读一般是检查不出来的
>     
> *   越界写如果是修改到标志位才会检查出来
>     
> 
> （系统在数组末尾后设的有标志位，越界写时，恰好修改到标志位了，就会被检查出来）

#### 12、修改指定下标位置的数据

```C
void SeqListAt(SeqList* psl, size_t pos, SLDataType x)
{
	assert(psl);  //断言
	assert(psl->size > 0);  //顺序表不能为空
	assert(pos >= 0 && pos < psl->size);  //检查pos下标的合法性

	psl->a[pos] = x;  //修改pos下标处对应的数据
}
```