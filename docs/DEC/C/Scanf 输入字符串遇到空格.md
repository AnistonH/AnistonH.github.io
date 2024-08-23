# 使用 scanf 时对空格处理？

*   问题描述
*   *   解决办法
*   总结

### 问题描述
----

scanf 输入字符串（含有空格的字符串，例如：“I love you!”）时，总是在空格处停止扫描。我们用 scanf("%s",str); 输入 “I love you!” 字符串后，str 输出却只有 “I” ，这并不是我们想要的。这是因为 scanf 扫描到 “I” 后面的空格，就认为对 str 的扫描结束（即空格没有被扫描），并舍弃后面的 "love you!"，只得到了 “I” 。

### 解决办法

在这里给出了两种解决办法，可以让空格也被扫描到 str 里。

1.  **gets() 函数** ，用 gets() 替代 scanf()；  
    gets 可以无限读取字符串，不会判断上限，以回车结束读取。其用法为 gets(s)，其中 s 为字符串变量（字符串数组名或字符串指针）。简单的理解就是读入一串字符（遇到回车结束），存到 s 中。

```c
#include<stdio.h>
int main()
{
   char str[50];
   gets(str);
   printf("%s\n",str);
   return 0;
}
```

2.  **scanf("%[^\n]",str)** ，遇到 "\n" 结束  
    '^'含有非的意思  
    “%[^\n]“即遇到 \ n 结束。  
    如果使用”%[^v]”，那我们输入 “I love you!” ，输出的就是 “I lo” ，现在能懂这个非的意思了吧…

```c
#include<stdio.h>
int main()
{
	char str[50];
	scanf("%[^\n]",str);//"%[^v]"
	printf("%s\n",str);
	return 0;
}
```
