#  [qsort](https://so.csdn.net/so/search?q=qsort&spm=1001.2101.3001.7020)（）函数

 [qsort](https://so.csdn.net/so/search?q=qsort&spm=1001.2101.3001.7020)（）函数（quick sort）是八大排序算法中的快速排序，能够排序任意数据类型的数组其中包括整形，浮点型，字符串甚至还有自定义的结构体类型。

## 1. 参数含义

-------

```
int cmp_int(const void* e1, const void* e2)
{
	return *(int*)e1 - *(int*)e2;
}
```

### 1.1 首元素地址 base

我们要排序一组数据，首先我们需要找到这组数据在哪，因此我们直接将首元素的地址传给 qsort 函数来确定从哪开始排序。

### 1.2 元素个数 num

我们知道了从哪开始，也要知道在哪结束才能确定一组需要排序的数据，但是我们不方便直接将结尾元素的地址传入函数，因此我们将需要排序的元素的个数传给 qsort 函数来确定一组数据。

### 1.3 元素大小 size

我们知道 qsort 函数能排序任意数据类型的一组数据，因此我们用 void * 类型的指针来接收元素，但是我们知道 void * 类型的指针不能进行加减操作，也就无法移动，那么在函数内部我们究竟用什么类型的指针来操作变量呢？我们可以将 void * 类型的指针强制类型转换成 char * 类型的指针后来操作元素，因为 char * 类型的指针移动的单位字节长度是 1 个字节，我们只需要再知道我们需要操作的数据是几个字节就可以操作指针从一个元素移动到下一个元素，因此我们需要将元素大小传入 qsort 函数。

### 1.4 自定义比较函数 compar

我们需要告诉 qsort 函数我们希望数据按照怎么的方式进行比较，比如对于几个字符串，我们可以比较字符串的大小（strcmp），也可以比较字符串的长度（strlen），因此我们要告诉 qsort 函数我们希望的比较方式，我们就需要传入一个比较函数 compar 就简写为 cmp 吧。


## 2. 使用方式
-------

### 2.1 头文件

![](https://img-blog.csdnimg.cn/20210909211942978.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAWkREV0xJRw==,size_20,color_FFFFFF,t_70,g_se,x_16)

 要使用 qsort 函数我们首先需要引用一个头文件 <stdlib,h>

```
int cmp_float(const void* e1, const void* e2)
{
	return (int)(*(float*)e1 - *(float*)e2);
}
```

### 2.2compar 的实现

qsort 函数给 cmp 函数规定了特定的参数。因此我们设计 cmp 函数时要严格遵守其参数设定。

```
int cmp_str_size(const void* e1, const void* e2)
{
	return strcmp((char*)e1,(char*)e2);
}
```

如果你要比较的数据是整形：

```
int cmp_str_len(const void* e1, const void* e2)
{
	return strlen((char*)e1)-strlen((char*)e2);
}
```

如果你要比较的数据是[浮点型](https://so.csdn.net/so/search?q=%E6%B5%AE%E7%82%B9%E5%9E%8B&spm=1001.2101.3001.7020)：

```
int cmp_by_age(const void*e1, const void*e2)
{
	return (int)(((stu*)e1)->weight - ((stu*)e2)->weight);
}
```

如果你要比较的是字符串的大小：

```
#include <stdlib.h>
typedef struct stu
{
	//char name;
	int age;
	float weight;
	double hight;
}stu;
int cmp_by_age(const void*e1, const void*e2)
{
	return (int)(((stu*)e1)->weight - ((stu*)e2)->weight);
}
int main()
{
	stu class1[3] = { {17,185.5,190.8}, {16,160.9,200.7}, {18,120.3,150.5} };
	int sz = sizeof(class1) / sizeof(class1[0]);
	int i;
	qsort(class1, sz,sizeof(class1[0]), cmp_by_age);
	for (i = 0; i < 3; i++)
	{
		printf("%.1f\n", class1[i].weight);
	}
	return 0;
}
```

如果你要比较的是字符串的长度：

```
int cmp_str_len(const void* e1, const void* e2)
{
	return strlen((char*)e1)-strlen((char*)e2);
}
```

如果你要比较的数据是结构体变量：

```
int cmp_by_age(const void*e1, const void*e2)
{
	return (int)(((stu*)e1)->weight - ((stu*)e2)->weight);
}
```

 需要注意的是：返回结果一定要确保是整形，如果不是一定要强制类型转换成整形！

## 3. 整体代码
-------

快速排序结构体变量示例：

```
#include <stdlib.h>
typedef struct stu
{
	//char name;
	int age;
	float weight;
	double hight;
}stu;
int cmp_by_age(const void*e1, const void*e2)
{
	return (int)(((stu*)e1)->weight - ((stu*)e2)->weight);
}
int main()
{
	stu class1[3] = { {17,185.5,190.8}, {16,160.9,200.7}, {18,120.3,150.5} };
	int sz = sizeof(class1) / sizeof(class1[0]);
	int i;
	qsort(class1, sz,sizeof(class1[0]), cmp_by_age);
	for (i = 0; i < 3; i++)
	{
		printf("%.1f\n", class1[i].weight);
	}
	return 0;
}
```