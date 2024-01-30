
### 1

**求二维数组中的最大值、最小值及所在的下标**

**编程实现**：求3行4列二维数组中的最大值、最小值及所在的下标。所有输入输出在主函数中完成，使用函数指针作为函数参数调用求最大值、最小值的功能函数。

```C
#include <stdio.h>
#define  M  3
#define  N  4
void fmax(int a[][N],int *l,int *r) {
    int i,j;
    for(i=0; i<M; i++)
        for(j=0; j<N; j++)
            if(a[i][j]>a[*l][*r]) {
                *l=i;
                *r=j;
            }
}
void fmin(int a[][N],int *l,int *r) {
    int i,j;
    for(i=0; i<M; i++)
        for(j=0; j<N; j++)
            if(a[i][j]<a[*l][*r]) {
                *l=i;
                *r=j;
            }
}
void pro(int a[][N],int *l,int *r,void (*f)(int [][N],int *,int *)) {
    f(a,l,r);
}
int main( ) {
    int a[M][N],i,j,line=0,row=0;
    printf("请输入一个%d行%d列的数组：",M,N);
    for(i=0; i<M; i++)
        for(j=0; j<N; j++)
            scanf("%d",&a[i][j]);
    pro(a,&line,&row,fmax);
    printf("最大值=%d,其下标为%d %d\n",a[line][row],line,row);
    pro(a,&line,&row,fmin);
    printf("最小值=%d,其下标为%d %d\n",a[line][row],line,row);
    return 0;
}
```



### 2

**求二维数组中的最大值和其地址**

**编程实现**：设计一个函数，找出3行3列的二维数组中的最大值和其地址，通过形参传回最大值，而最大值的地址由该函数return语句返回。在主函数中输出数组首址、最大值和其地址。

```
数组首址=0x7ffc9a9b1250,最大值=423,其地址=0x7ffc9a9b1258
```

```C
#include <stdio.h>
#define  N  3
#define  M  3
int *select(int a[N][M],int *n) {
    int i,j,row=0, colum=0;
    for(i=0; i<N; i++)
        for(j=0; j<M; j++)
            if(a[i][j]>a[row][colum]) {
                row=i;
                colum=j;
            }
    *n=a[row][colum];
    return a[0]+row*N+colum;
}
int main( ) {
    int a[N][M]= {99,311,423,6,1,15,9,17,20},max,*p;
    p=select(a,&max);
    printf("数组首址=%p,最大值=%d,其地址=%p\n",a[0],max,p);
    return 0;
}
```



### 3

**将n个整数，循环后移m个位置**

**编程实现**：有8个整数，使前面各数顺序向后移动m个位置，最后m个数变成最前面m个数。写一函数实现上述功能，在主函数中输入N个整数和输出调整后的N个数。

```C
#include <stdio.h>
void cyc(int *p,int n,int m);
//将n个整数，循环后移m个位置
#define N 8
void cyc(int *p,int n,int m) {
    int i,k,temp;
    for(k=0; k<m; k++) {	//循环移位m次
        temp=*(p+n-1);	//暂存最后一个元素，空出那个位置
        for(i=0; i<n-1; i++)
            *(p+n-i-1)=*(p+n-i-2);//依次做：空出的位置保存前一个元素
        *p=temp;		//最前面的位置保存暂存的元素----循环移位一次
    }
}
int main() {
    int n=N,m,a[N],*p=a,i;
    printf("请输入%d个整数:",N);
    for(i=0; i<N; i++)
        scanf("%d",p+i);
    printf("向后移动几个位置?");
    scanf("%d",&m);
    cyc(p,n,m);				//循环后移
    printf("移动后的数据为：\n");
    for(i=0; i<n; i++)
        printf("%-4d",*(p+i));
    printf("\n");
    return 0;
}
```



### 4

**使用指针判断回文数**

**编程实现**：设计一个函数`int fun(char *p1)`，功能是：判别字符串str是否为“回文”，若是返回1，否则返回0。例如，“12321”、“abcdcba”是回文，而“123”、“hello”不是。

```C
#include<stdio.h>
#include<string.h>
int fun(char *p1) {
    //判断一个字符串是否为回文
    int flag=1;
    char *p2;
    p2=p1+strlen(p1)-1;
    while(p2>p1)
        if(*p2--!=*p1++) {
            flag=0;
            break;
        }
    return flag;
}
int main() {
    char s[100];
    int flag;
    printf("请输入一个字符串:\n");
    gets(s);
    flag=fun(s);
    printf("%s\n",flag?"1":"0");
    return 0;
}
```



### 5

**循环报数**

**编程实现**：有n人围成一个圆圈，分别编号1~n，从第1人到m循环报数，凡是报到m者离开圆圈，求n个人离开圆圈的次序。

```C
#include<stdio.h>
void fun(int *p,int n,int m) {
    int i,j,k;
    i=1;					//循环计人数
    k=0;				//离开人数计数
    j=0;					//报数计数
    while(k<n) {
        if(*(p+i)!=0)		//!=0表示有人
            j++;			//报数
        if( j==m) {		//报到m者离开，置其为0
            printf("%-5d",*(p+i));
            *(p+i)=0;
            j=0;			//为重新报数置初值
            k++;			//离开者计数
        }
        i++;
        if(i==n+1)
            i=1;
    }
}
int main( ) {
    int i,m,n,array[100],*p;
    p=array;
    printf("请输入人数和报数:\n");
    scanf("%d%d",&n,&m);
    for(i=1; i<=n; i++)
        *(p+i)=i;
    printf("离开的顺序为 :\n");
    fun(p,n,m);
    printf("\n");
    return 0;
}
```



### 6

**找出数组中的最小值**

**编程实现**：有一个数组，内放10个整数，要求编写函数`int processor(int *p)`找出最小的数和它的下标，然后把它和数组中最前面的元素调换，下标返回给主函数输出，原始数组和改变后的数组由`void output(int *p)`输出。

```
最小数的下标为：9
改变后的数组为:
   -1   13   45   10    8    9    7   30   21    0
```

```C
#include<stdio.h>
#define N 10
//查找最小数及其下标,并和第一个数交换
int processor(int *p) {
    int  *p1,i,j,data;
    p1=p;
    j=0;
    for(i=1; i<N; i++) {
        if(*p1>*(p+i)) {
            p1=p+i;
            j=i;
        }
    }
    if(j!=0) {
        data=*p;//如果第一个数不是最小的，将最小数和第一个数交换
        *p=*p1;
        *p1=data;
    }
    return j;
}
void output(int *p) {
    int i;
    for(i=0; i<N; i++) {
        printf("%5d",*p);
        p++;
    }
    printf("\n");
}
int main() {
    int array[N]= {0,13,45,10,8,9,7,30,21,-1};
    int *p=array;
    printf("原始数组为:\n");
    output(p);
    printf("最小数的下标为：%d\n",processor(p));
    printf("改变后的数组为:\n");
    output(p);
    return 0;
}
```



### 7

**求输入10个整数中的正数之和及个数**

**编程实现**：求输入10个整数中的正数之和及个数。要求：编写一个函数完成计算和统计，在主函数中完成所有的输入输出。

```C
#include <stdio.h>
#define N 10
void count(int a[],int *plus,int *sum) {
    int i;
    for(i=0; i<N; i++) {
        if (a[i]>0) {
            (*plus)++;
            (*sum)+=a[i];
        }
    }
}
int main() {
    int i,a[N],plus=0,sum=0;
    printf("请输入%d个整数：",N);
    for(i=0; i<N; i++)
        scanf("%d",&a[i]);
    count(a,&plus,&sum);
    printf("正数有%d个，正数之和为:%d\n",plus,sum);
    return 0;
}
```



### 8

**使用指针统计字符串中各字符数量**

**编程实现**：分别统计字符串中大写字母、小写字母、空格及数字字符的个数。要求：在主函数中完成字符串的输入及输出统计结果，在函数`void count(char a[],int *upper,int *lower,int *space,int *digit)`中实现统计。请按照大写字母、小写字母、空格及数字的顺序进行统计

```C
#include "stdio.h"
void count(char a[],int *upper,int *lower,int *space,int *digit) {
    int i;
    for(i=0; a[i]!='\0'; i++) {
        if ('A'<=a[i] && a[i]<='Z')
            (*upper)++;      /*统计大写字母的个数*/
        else if ('a'<=a[i] && a[i]<='z')
            (*lower)++;      /*统计小写字母的个数*/
        else if ('0'<=a[i] && a[i]<='9')
            (*digit)++;      /*统计数字的个数*/
        else if (a[i]==' ')
            (*space)++;      /*统计空格的个数*/
    }
}
int main() {
    char a[81];
    int upper=0,lower=0, space=0,digit=0;
    printf("请输入一个字符串：");
    gets(a);
    count(a,&upper,&lower,&space,&digit);
    printf("大写字母、小写字母、空格和数字的个数分别为：%d %d %d %d\n",upper,lower,space,digit);
    return 0;
}
```



### 9

**使用指针对两个整数进行排序**

**编程实现**：输入两个整数，按由大到小顺序输出。要求：在主函数中完成整数的输入输出，在函数`void cmp(int *pmax,int *pmin)`中实现两个整数的比较和交换。

```C
#include "stdio.h"
void cmp(int *pmax,int *pmin) {
    int t;
    if(*pmin>*pmax) {
        t=*pmin;
        *pmin=*pmax;
        *pmax=t;
    }
}
int main() {
    int pmax,pmin;
    printf("请输入两个整数：");
    scanf("%d%d",&pmax,&pmin);
    cmp(&pmax,&pmin);
    printf("按大小顺序输出的两个整数为：%d %d\n",pmax,pmin);
    return 0;
}
```



### 10

**使用指针统计N阶方阵**

**编程实现**：求N阶方阵所有元素之和、主对角线元素之和、次对角线元素之和。要求设计一个功能菜单，用户根据菜单进行功能选择，程序根据用户的选择使用函数指针作为函数参数调用相应的功能函数。

```C
#include "stdio.h"
//#include "conio.h"
#include "stdlib.h"
#define N 4
void Sum(int a[][N])
{
    int i, j, sum = 0;
    for (i = 0; i < N; i++)
        for (j = 0; j < N; j++)
            sum = sum + a[i][j];
    printf("所有元素之和为：%d\n", sum);
}
void SumM(int a[][N])
{
    int i, summ = 0;
    for (i = 0; i < N; i++)
        summ = summ + a[i][i];
    printf("主对角线元素之和为：%d\n", summ);
}
void SumS(int a[][N])
{
    int i, sums = 0;
    for (i = 0; i < N; i++)
        sums = sums + a[i][N - 1 - i];
    printf("次对角线元素之和为：%d\n", sums);
}
void Pro(int a[][N], void (*fun)(int a[][N]))
{
    //调用各功能函数
    (*fun)(a);
}
void ShowMenu()
{
    //显示菜单
    printf("\t***************************************\n");
    printf("\t*\t                              *\n");
    printf("\t*\t\t功能菜单              *\n");
    printf("\t*\t1.求所有元素之和              *\n");
    printf("\t*\t2.求主对角线元素之和	      *\n");
    printf("\t*\t3.求次对角线元素之和	      *\n");
    printf("\t*\t0.退出	                      *\n");
    printf("\t***************************************\n");
}
int Select(int i, int a[][N])
{
    //菜单选择
    switch (i)
    {
    case 1:
        Pro(a, Sum);
        break;
    case 2:
        Pro(a, SumM);
        break;
    case 3:
        Pro(a, SumS);
        break;
    case 0:
        printf("感谢使用，再见！\n");
    }
    if (i)
    {
        printf("按任意键继续……\n");
        getchar();
    }
    return !i;
}
int main()
{
    int i, a[N][N] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16};
    while (1)
    {
        system("clear");
        ShowMenu();
        printf("请输入你的选择（1,2,3,0）：");
        scanf("%d", &i);
        fflush(stdin); 
        if (i >= 0 && i <= 3)
            if (Select(i, a))
                break;
    }
    return 0;
}
```



### 11

**使用指针实现简易计算器	**

**编程实现**：编写一个简易计算器程序，实现整数的加法、减法、乘法运算。要求设计一个功能菜单，用户根据菜单进行功能选择，程序根据用户的选择使用函数指针作为函数参数调用相应的功能函数。

```C
/*
 有两个版本供参考：
 版本1: 可以通过测试的版本，但没有题干中描述的输出信息，不使用无限循环
 版本2: 显示更多信息，像题干描述的那样。但需要在本地运行，因为使用了无限循环
*/

/************** 版本1 **************/ 
#include "stdio.h"
//#include "conio.h"  conio.h不是标准库头文件，平台不支持，如需使用，请在本地编译器调试
#include <stdlib.h>
int Sum(int a,int b) {
    //加法
    return a+b;
}
int Sub(int a,int b) {
    //减法
    return a-b;
}
int Mul(int a,int b) {
    //乘法
    return a*b;
}
int Pro(int a,int b,int (*fun)(int,int)) {
    //调用各功能函数
    return (*fun)(a,b);
}
void ShowMenu() {
    //显示菜单
    printf("\t*********************************\n");
    printf("\t*	    简易计算器		*\n");
    printf("\t*	1.加法	    2.减法	*\n");
    printf("\t*	3.乘法	    0.退出	*\n");
    printf("\t*********************************\n");
}
int Select(int i) {
    //菜单选择
    int x,y,(*str[3])(int,int)= {Sum,Sub,Mul};
    char Op[3]= {'+','-','*'};
    if(i) {
        //printf("请输入两个整数，以空格分开：");
        scanf("%d%d",&x,&y);
        printf("%d %c %d = %d\n",x,Op[i-1],y,Pro(x,y,str[i-1]));
    } else {
        //printf("感谢使用本计算器，再见！\n");
        return 1;
    }
    //printf("按任意键继续……");
    //getchar();
    return 0;
}
int main() {
    int i;
//        ShowMenu();
//        printf("请输入你的选择（1,2,3,0）：");
        scanf("%d",&i);   
        if(i>=0&&i<=3)
            if(Select(i))
                return 0;
    return 0;
}
/************** 版本1 结束 **************/



/************** 版本2 **************/
#include "stdio.h"
//#include "conio.h"  conio.h不是标准库头文件，平台不支持，如需使用，请在本地编译器调试
#include <stdlib.h>
int Sum(int a,int b) {
    //加法
    return a+b;
}
int Sub(int a,int b) {
    //减法
    return a-b;
}
int Mul(int a,int b) {
    //乘法
    return a*b;
}
int Pro(int a,int b,int (*fun)(int,int)) {
    //调用各功能函数
    return (*fun)(a,b);
}
void ShowMenu() {
    //显示菜单
    printf("\t*********************************\n");
    printf("\t*	    简易计算器		*\n");
    printf("\t*	1.加法	    2.减法	*\n");
    printf("\t*	3.乘法	    0.退出	*\n");
    printf("\t*********************************\n");
}
int Select(int i) {
    //菜单选择
    int x,y,(*str[3])(int,int)= {Sum,Sub,Mul};
    char Op[3]= {'+','-','*'};
    if(i) {
        printf("请输入两个整数，以空格分开：");
        scanf("%d%d",&x,&y);
        printf("\t%d %c %d = %d\n",x,Op[i-1],y,Pro(x,y,str[i-1]));
    } else {
        printf("感谢使用本计算器，再见！\n");
        return 1;
    }
    printf("按任意键继续……");
    getchar();
    return 0;
}
int main() {
    int i;
    while(1) {
        system("clear");
        ShowMenu();
        printf("请输入你的选择（1,2,3,0）：");
        scanf("%d",&i);   
        if(i>=0&&i<=3)
            if(Select(i))
                break;
    }
    return 0;
}
/************** 版本2 结束 **************/
```



### 12

**使用指针实现字符串复制**

**编程实现**：编写函数void StrCopy(char *ps1, char *ps2)，其功能：把字符串ps2的内容复制到字符串ps1中，要求不能使用strcpy函数。主函数输入两个字符串和调用StrCopy函数输出复制后ps1的字符串。

```C
#include "stdio.h"
#define N 30
void StrCopy(char *ps1, char *ps2) {
    while(*ps2)
        *ps1++=*ps2++;
    *ps1='\0';
}
int main() {
    char str1[N],str2[N];
    printf("请输入两个字符串：\n");
    scanf("%s%s",str1,str2);
    StrCopy(str1,str2);
    printf("复制后的字符串为：\n");
    puts(str1);
    return 0;
}
```



### 13

**使用指针实现字符串连接**

**编程实现**：编写函数`void StrCat(char *ps1, char *ps2)`将字符串ps2连接到字符串ps1的后面。主函数输入两个字符串和调用StrCat函数输出连接后的字符串

```C
#include "stdio.h"
#define N 30
void StrCat(char *ps1,char *ps2) {
    for(; *ps1; ps1++);
    for(; *ps2; ps1++,ps2++)
        *ps1=*ps2;
    *ps1='\0';
}
int main() {
    char str1[N],str2[N];
    printf("请输入两个字符串：\n");
    scanf("%s%s",str1,str2);
    StrCat(str1,str2);
    printf("连接后的字符串为：\n");
    puts(str1);
    return 0;
}
```



### 14

**使用指针实现删除数组中指定元素**

**编程实现**：编写一个函数`void Delete(int *a,int i)`，用于实现删除数组a中第i个元素。

```C
#include "stdio.h"
#define N 10
void Delete(int *a,int i) {
    for(; i<N-1; i++)
        *(a+i)=*(a+i+1);
}
int main() {
    int a[N],i,n;
    printf("请输入%d个整数:",N);
    for(i=0; i<N; i++)
        scanf("%d",&a[i]);
    printf("请输入删除数组中第几个元素：");
    scanf("%d",&n);
    Delete(a,n);
    printf("删除数组中第%d个元素后的数据为：\n",n);
    for(i=0; i<N-1; i++)
        printf("%d ",a[i]);
    printf("\n");
    return 0;
}
```



### 15

**使用指针实现有序数组的插入**

**编程实现**：使用指针在按升序排序的数组中插入一个数据x，使插入后的数组仍然有序。

```C
#include "stdio.h"
#define N 10
void Insert(int *p, int x) {
    //在有序的数组中插入一个数据，插入后的数组仍然有序
    int i,j;
    for(i=0; i<N-1 && *(p+i)<x; i++);	//查找数据x应插入的位置i
    for(j=N-2; j>=i; j--)	//将下标为i到n-1的所有元素后移一位
        p[j+1]=p[j];
    p[i]=x;	//将数据x插入到数组中
}
void Output(int a[],int n) {
    //输出数组的全部元素
    int i;
    for(i=0; i<n; i++)
        printf("%4d",a[i]);
    printf("\n");
}
int main() {
    int a[N]= {1,4,5,7,19,20,34,56,78},x;
    printf("请输入要插入的数据：");
    scanf("%d",&x);
    printf("原数组为：\n");
    Output(a,N-1);	//输出数据插入前的全部数组元素
    Insert(a, x);		//插入数据
    printf("插入%d后的数组为：\n",x);
    Output(a,N);		//输出数据插入后的全部数组元素
    return 0;
}
```



### 16

**使用指针求字符串中最大字符的地址**

**编程实现**：编写函数`char *MaxChar(char *str)`，求字符串str中最大字符的地址并返回该地址。

```C
#include "stdio.h"
#define N 50 
char *MaxChar(char *str) {
    char *maxp=str;
    while(*str++)
        if(*maxp<*str)
            maxp=str;
    return maxp;
}
int main()
{
	char str1[N],*max;
	printf("请输入字符串：");
	gets(str1);
	max=MaxChar(str1);
	printf("最大字符为：%c",*max); 
	return 0;
}
```



### 17

**使用指针输出对应的星期名**

**编程实现**：输入1～7之间的整数，输出对应的星期名。

```C
#include "stdio.h"
char *day_name(int n) {
    char *name[]= {"Illegal day","Monday","Tuesday","Wednesday", "Thursday","Friday","Saturday","Sunday"};
    return((n<1||n>7) ? name[0] : name[n]);
}
int main() {
    int i;
    printf("请输入一个整数(1-7)：");
    scanf("%d",&i);
    printf("%2d-->%s\n",i,day_name(i));
    return 0;
}
```



### 18

**求数组元素之和、最大值、最小值**

**编程实现**：编写函数`int Sum(int a[])`、`int Max(int a[])`、`int Min(int a[])`和`int Process(int a[],int (*fun)(int[]))`，分别求数组中10个元素之和、最大值、最小值，通过调用函数Process实现调用函数Sum、Max和Min。

```C
#include "stdio.h"
#define N 10
int Sum(int a[]) {
    int sum=0,i;
    for(i=0; i<N; i++)
        sum=sum+a[i];
    return sum;
}
int Max(int a[]) {
    int max=a[0],i;
    for(i=1; i<N; i++)
        if(a[i]>max)
            max=a[i];
    return max;
}
int Min(int a[]) {
    int min=a[0],i;
    for(i=1; i<N; i++)
        if(a[i]<min)
            min=a[i];
    return min;
}
int Process(int a[],int (*fun)(int *)) {
    return (*fun)(a);
}
int main() {
    int a[N],i;
    printf("请输入%d个整数:",N);
    for(i=0; i<N; i++)
        scanf("%d",&a[i]);
    printf("元素之和:%d\n",Process(a,Sum));
    printf("最大值:%d\n",Process(a,Max));
    printf("最小值:%d\n",Process(a,Min));
    return 0;
}
```



### 19

**使用指针求两个整数的最大值和最小值**

**编程实现**：编写函数`int Max(int a,int b)`、`int Min(int a,int b)`和`int Process(int a,int b,int (*fun)(int,int))`，分别求两个整数a与b的最大值、最小值，通过调用函数Process实现调用函数Max和Min。

```C
#include "stdio.h"
int Max(int a,int b) {
    return a>b?a:b;
}
int Min(int a,int b) {
    return a<b?a:b;
}
int Process(int a,int b,int (*fun)(int,int)) {
    return (*fun)(a,b);
}
int main() {
    int a,b,max,min;
    printf("请输入两个整数:\n");
    scanf("%d%d",&a,&b);
    max=Process(a,b,Max);
    printf("最大值:%d\n",max);
    min=Process(a,b,Min);
    printf("最小值:%d\n",min);
    return 0;
}
```



### 20

**使用指针求一组整数的最大值和最小值**

**编程实现**：求一组5个整数的最大值和最小值。要求：编写函数`void Input(int a[])`实现一组整数的输入，编写函数`int Maxmin(int a[],int *pmax,int *pmin)`实现求一组整数的最大值和最小值。

```C
#include "stdio.h"
#define N 5
void Input(int a[]) {
    int i;
    printf("请输入%d个整数：\n",N);
    for(i=0; i<N; i++)
        scanf("%d",&a[i]);
}
int Maxmin(int a[],int *pmax,int *pmin) {
    int i;
    *pmax=*pmin=a[0];
    for(i=1; i<N; i++) {
        if(a[i]>*pmax)
            *pmax=a[i];
        if(a[i]<*pmin)
            *pmin=a[i];
    }
    return 0;
}
int main() {
    int a[N],pmax,pmin;
    Input(a);
    Maxmin(a,&pmax,&pmin);
    printf("最大值为：%d,最小值为：%d\n",pmax,pmin);
    return 0;
}
```



### 21

**交换形参指针变量的指向**

**编程实现**：使用指针交换形参指针变量的指向，数据由主函数输入。

```C
    #include "stdio.h"
    void swap1(int *p1,int *p2) {
        int *temp;
        printf("交换前%d,%d\n",*p1,*p2);//交换前p1指向a，p2指向b
        temp=p1;
        p1=p2;
        p2=temp;
        printf("交换后%d,%d\n",*p1,*p2);//交换后p1指向b，p2指向a
    }
    int main() {
        int a,b,*p=&a,*q=&b;
        printf("请输入两个整数：");
        scanf("%d%d",p,q);
        swap1(p,q);
        return 0;
    }
```



### 22

**使用指针交换实参变量的值**

**编程实现**：使用指针交换实参变量的值，实参变量为整型数据，由主函数输入。

```C
#include "stdio.h"
void swap(int *p1,int *p2) {
    int temp;
    temp=*p1;
    *p1=*p2;
    *p2=temp;
}
int main() {
    int a,b;
    printf("请输入两个整数：");
    scanf("%d%d",&a,&b);
    printf("交换前a=%d,b=%d\n",a,b);
    swap(&a,&b);
    printf("交换后a=%d,b=%d\n",a,b);
    return 0;
}
```



### 23

**求出数组的最大元素和最小元素**

编写程序，在主函数中定义一一维数组，另一个函数用来求出数组的最大元素和最小元素，但在主函数中输出。

```C
#include <stdio.h>
#define N 10
int fun(int *s,int *max) {
    int i,mm;
    mm=*max=*s;
    for(i=1; i<N; i++) {
        if(*(s+i)>*max)
            *max=*(s+i);
        if(*(s+i)<mm)
            mm=*(s+i);
    }
    return mm;
}
int main() {
    int a[N]= {876,675,896,101,301,401,980,431,451,777},max,min;
    min=fun(a,&max);
    printf("最大元素=%d,最小元素=%d\n",max,min);
    return 0;
}
```



### 24

**将输入的一个正整数中每一位上为奇数的数依次取出，构成一个新数输出**

将输入的一个正整数中每一位上为奇数的数依次取出，构成一个新数输出。高位仍在高位，低位仍在低位。

```C
#include <stdio.h>
void fun(int s,int *t) {
    int d,s1=1;
    *t=0;
    while(s>0) {
        d=s%10;
        if(d%2!=0) {
            *t=d*s1+*t;
            s1*=10 ;
        }
        s/=10;
    }
}
int main() {
    int s,t;
    printf("请输入一个正整数: ");
    scanf("%d",&s);
    fun(s,&t);
    printf("t=%d\n",t);
    return 0;
}
```



### 25

**将其余字符按ASCII值码升序排列**

输入一个字符串（长度不超过80个字符），除首、尾字符外，请将其余字符按ASCII值码升序排列。例如：原来的字符串为BdsihAd，则排序后输出为BAdhisd。

```C
#include <stdio.h>
#include <string.h>
void fun(char *s, int num) {
    char t;
    int i, j;
    for(i=1; i<num-2; i++)	/*下标从1开始，用循环依次取得字符串中的字符*/
        for(j=i+1; j<num-1; j++) /*将字符与其后的每个字符比较*/
            if(s[i]>s[j]) {
                t=s[i]; /*则交换这两个字符*/
                s[i]=s[j];
                s[j]=t;
            }
}
int main() {
    int i;
    char s[81];
    printf("请输入一个字符串:");
    gets(s);
    i=strlen(s);
    fun(s,i);
    printf("排序后的字符串是:%s\n",s);
    return 0;
}
```



### 26

**使用指针法完成两个超长正整数的加法	**

使用指针法完成两个超长（如20位）正整数的加法。

```C
#include "stdio.h"
void fun1(int *p1,int *q1,int i,int j) { //把最低位对齐
    int t,*p2=p1+i,*q2=q1+j;
    if(i>j) {	//b[]短
        t=i-j;
        while(q2>=q1) {
            *(q2+t)=*q2;
            q2--;
        }
        for(; t>=0; t--)		//高位部分清0
            *q1++=0;
    }
    if(i<j) {	//a[]短
        t=j-i;
        while(p2>=p1) {
            *(p2+t)=*p2;
            p2--;
        }
        for(; t>=0; t--)		//高位部分清0
            *p1++=0;
    }
}
void add(int *p1,int *q1,int i) {
    int c=0,*p2,*q2,t;	//c进位标志
    p2=p1+i-1;
    q2=q1+i-1;
    while(p2>=p1) {
        t=((*p2)+(*q2)+c)%10;	//个位
        c=((*p2)+(*q2)+c)/10;	//进位
        *p2=t;
        p2--;
        q2--;
    }
    *p2=c;
}
int main() {
    int a[201]= {0},b[201]= {0},*p1=a,*q1=b,i=0,j=0;
    printf("请输入第一个正整数:");
    do {	//a[0]=0,可存最后的进位
        *(p1+(++i))=getchar()-'0'; // \n-'0'=-38
    } while(*(p1+i)!=-38);
    printf("请输入第二个正整数:");
    do {
        *(q1+(++j))=getchar()-'0';
    } while(*(q1+j)!=-38);
    fun1(a,b,i,j);		//把最低位对齐
    if(i<j)
        i=j;		//i＝长数的个数
    add(a,b,i);
    printf("和为：");
    for(; i>0; i--)
        printf("%d",*p1++);
    printf("\n");
    return 0;
}
```



### 27

**输入字符串s和t，检查字符串s中是否包含字符串t**

输入字符串s和t，检查字符串s中是否包含字符串t，若包含，则返回t在s中的开始位置（位置值从0开始计算），否则送回-1。要求用指针方式实现。

```C
#include <stdio.h>
index(char *s, char *t) {
    char *p,*h=s,*tmp=t;
    for(; *s; s++ ) {
        for(p=s; *t && *t==*p; p++,t++)
            ;
        if(!(*t))
            return (s-h);
        else
            t=tmp;
    }
    return(-1);
}
int main() {
    int n;
    char s[81],t[81];
    printf("请输入字符串s：");
    gets(s);
    printf("请输入字符串t：");
    gets(t);
    n=index(s,t);
    printf("%d\n",n);
    return 0;
}
```



### 28

**用指针法将字符串中指定的字符删除。**

用指针法将字符串中指定的字符删除。

```C
#include "stdio.h"
void del( char *s,char c ) {
    char *p=s;
    for(; *s; s++)
        if(*s!=c)
            *p++=*s;
    *p='\0';
}
int main() {
    char a[81],c;
    printf("请输入字符串：");
    gets(a);
    printf("请输入要删除的字符：");
    c=getchar();
    del(a,c);
    puts(a);
    return 0;
}
```



### 29

**指向指针的指针对整数排序**

用指向指针的指针的方法对 n 个整数排序并输出。要求将排序单独写成一个函数。n 个整数在主函数中输入，最后在主函数中输出。

```C
#include <stdio.h>

void sort (int **p, int n) {
    int i, j, *temp;
    for (i = 0; i < n - 1; i++) {
        for (j = i + 1; j < n; j++) {
            if (**(p + i) > **(p + j)) {
                temp = *(p + i);
                *(p + i) = *(p + j);
                *(p + j) = temp;
            }
        }
    }
}

int main () {
    int i, n, data[20], **p, *pstr[20];
    scanf("%d", &n);
    for (i = 0; i < n; i++)
        pstr[i] = &data[i];
    for (i = 0; i < n; i++)
        scanf("%d", pstr[i]);
    p = pstr;
    sort(p, n);
    for (i = 0; i < n; i++)
        printf("%d  ", *pstr[i]);
    printf("\n");
    return 0;
}
```



### 30

**指向指针的指针对字符串排序**

用指向指针的指针的方法对 5 个字符串排序并输出。

```C
#include <stdio.h>
#include <string.h>
#define LINEMAX 20

void sort (char **p) {
    int i, j;
    char *temp;
    for (i = 0; i < 5; i++) {
        for (j = i + 1; j < 5; j++) {
            if (strcmp(*(p + i), *(p + j)) > 0) {
                temp = *(p + i);
                *(p + i) = *(p + j);
                *(p + j) = temp;
            }
        }
    }
}

int main () {
    int i;
    char **p, *pstr[5], str[5][LINEMAX];
    for (i = 0; i < 5; i++)
        pstr[i] = str[i];
    for (i = 0; i < 5; i++)
        scanf("%s", pstr[i]);
    p = pstr;
    sort(p);
    for (i = 0; i < 5; i++)
        printf("%s\n", pstr[i]);
    return 0;
}
```



### 31

**比较两个字符串**

从键盘输入两个字符串 s1、s2，实现两个字符串的比较。

- 编写一个 strcmp 函数实现两个字符串的比较，函数原型为`strcmp(char *p1, char *p2)`，
- 设 p1 指向字符串 s1，p2 指向字符串 s2。
- 要求当 s1 = s2 时，返回值为 0；
- 当 s1 ≠ s2 时，返回它们二者第 1 个不同字符的 ASCII 码差值(如"BOY"与"BAD"，第 2 个字母不同，"O"与"A"之差为 79 - 65 = 14)；
  - 当 s1 > s2 时，则输出正值；
  - 当 s1 < s2 时，则输出负值。

输入示例

```clike
CHINA
Chen
```

输出示例

```clike
result: -32
```

```C
#include <stdio.h>
int strcmp (char *p1, char *p2) {
    int i;
    i = 0;
    while (*(p1 + i) == *(p2 + i))
        if (*(p1 + i++) == '\0')
            return 0;
    return *(p1 + i) - *(p2 + i);
}

int main () {
    int m;
    char str1[20], str2[20];
    gets(str1);
    gets(str2);
    m = strcmp(str1, str2);
    printf("result: %d\n", m);
    return 0;
}
```



### 32

**统计字符串内数字和非数字	**

输入一个字符串，内有数字和非数字字符，例如`A123x456 17960? 302tab5976`，将其中连续的数字作为一个整数，依次存放到一数组 a 中。例如，123 放在 a[0]，456 放在 a[1]…。统计共有多少个整数，并输出这些数。

```C
#include <stdio.h>

int main () {
    char str[50], *pstr;
    int i, j, k, m, e10, digit, ndigit, a[10], *pa;
    gets(str);
    pstr = &str[0];
    pa = &a[0];
    ndigit = 0;
    i = 0;
    j = 0;
    while (*(pstr + i) != '\0') {
        if ((*(pstr + i) >= '0') && (*(pstr + i) <= '9'))
            j++;
        else {
            if (j > 0) {
                digit = *(pstr + i - 1) - 48;
                k = 1;
                while (k < j) {
                    e10 = 1;
                    for (m = 1; m <= k; m++)
                        e10 = e10 * 10;
                    digit = digit + (*(pstr + i - 1 - k) - 48) * e10;
                    k++;
                }
                *pa = digit;
                ndigit++;
                pa++;
                j = 0;
            }
        }
        i++;
    }
    if (j > 0) {
        digit = *(pstr + i - 1) - 48;
        k = 1;
        while (k < j) {
            e10 = 1;
            for (m = 1; m <= k; m++)
                e10 = e10 * 10;
            digit = digit + (*(pstr + i - 1 - k) - 48) * e10;
            k++;
        }
        *pa = digit;
        ndigit++;
        j = 0;
    }
    printf("There are %d numbers in this line, they are: \n", ndigit);
    j = 0;
    pa = &a[0];
    for (j = 0; j < ndigit; j++)
        printf("%d ", *(pa + j));
    printf("\n");
    return 0;
}
```



### 33

**课程的相关统计**

有一个班 4 个学生，5 门课程。

① 求第 1 门课程的平均分；

② 找出有两门以上课程不及格的学生，输出他们的学号和全部课程成绩及平均成绩；

③ 找出平均成绩在 90 分以上或全部课程成绩在 85 分以上的学生。

分别编 3 个函数实现以上 3 个要求。

```C
#include <stdio.h>

void avsco (double *pscore, double *paver) {
    int i, j;
    double sum, average;
    for (i = 0; i < 4; i++) {
        sum = 0.0;
        for (j = 0; j < 5; j++)
            sum = sum + (*(pscore + 5 * i + j));
        average = sum / 5;
        *(paver + i) = average;
    }
}

void avcour1 (char(*pcourse)[10], double *pscore) {
    int i;
    float sum, average1;
    sum = 0.0;
    for (i = 0; i < 4; i++)
        sum = sum + (*(pscore + 5 * i));
    average1 = sum / 4;
    printf("course 1: %s average score: %7.2f\n", *pcourse, average1);
}

void fali2 (char course[5][10], int num[], double *pscore, double aver[4]) {
    int i, j, k, labe1;
    printf("No.");
    for (i = 0; i < 5; i++)
        printf("%10s", course[i]);
    printf("   average\n");
    for (i = 0; i < 4; i++) {
        labe1 = 0;
        for (j = 0; j < 5; j++)
            if (*(pscore + 5 * i + j) < 60.0)
                labe1++;
        if (labe1 >= 2) {
            printf("%d", num[i]);
            for (k = 0; k < 5; k++)
                printf("%10.2f", *(pscore + 5 * i + k));
            printf("%10.2f\n", aver[i]);
        }
    }
}

void good (char course[5][10], int num[4], double *pscore, double aver[4]) {
    int i, j, k, n;
    printf("No.");
    for (i = 0; i < 5; i++)
        printf("%10s", course[i]);
    printf("   average\n");
    for (i = 0; i < 4; i++) {
        n = 0;
        for (j = 0; j < 5; j++)
            if (*(pscore + 5 * i + j) > 85.0)
                n++;
        if ((n == 0) || (aver[i] >= 90)) {
            printf("%d", num[i]);
            for (k = 0; k < 5; k++)
                printf("%10.2f", *(pscore + 5 * i + k));
            printf("%10.2f\n", aver[i]);
        }
    }
}

int main () {
    int i, j, *pnum, num[4];
    double score[4][5], aver[4], *pscore, *paver;
    char course[5][10], (*pcourse)[10];
    pcourse = course;
    for (i = 0; i < 5; i++)
        scanf("%s", course[i]);
    pscore = &score[0][0];
    pnum = &num[0];
    for (i = 0; i < 4; i++) {
        scanf("%d", pnum + i);
        for (j = 0; j < 5; j++)
            scanf("%lf", pscore + 5 * i + j);
    }
    paver = &aver[0];
    avsco(pscore, paver);
    avcour1(pcourse, pscore);
    printf("\n");
    fali2(pcourse, pnum, pscore, paver);
    printf("\n");
    good(pcourse, pnum, pscore, paver);
    return 0;
}
```



### 34

**将多个数逆序排列**

将 n 个数按输入时顺序的逆序排列，用函数实现。

```C
#include <stdio.h>

void sort (char *p, int m) {
    int i;
    char temp, *p1, *p2;
    for (i = 0; i < m / 2; i++) {
        p1 = p + i;
        p2 = p + (m - 1 - i);
        temp = *p1;
        *p1 = *p2;
        *p2 = temp;
    }
}

int main () {
    int i, n;
    char *p, num[20];
    scanf("%d", &n);
    for (i = 0; i < n; i++)
        scanf("%d", &num[i]);
    p = &num[0];
    sort(p, n);
    for (i = 0; i < n; i++)
        printf("%d ", num[i]);
    printf("\n");
    return 0;
}
```



### 35

**求定积分的通用函数**

写一个用矩形法求定积分的通用函数，分别求![image.png](https://nuc.alphacoding.cn/api/resource/v2/exercises/5fe00414c975ca5b69c65cb6/f/N113O5XJMZXM)

```C
#include <stdio.h>
#include <math.h>

double integral (double (*p)(double), double a, double b, int n) {
    int i;
    double x, h, s;
    h = (b - a) / n;
    x = a;
    s = 0;
    for (i = 1; i <= n; i++) {
        x = x + h;
        s = s + (*p)(x) * h;
    }
    return s;
}

double fsin (double x) {
    return sin(x);
}

double fcos (double x) {
    return cos(x);
}

double fexp (double x) {
    return exp(x);
}

int main () {
    double a1, a2, a3, b1, b2, b3, c, (*p)(double);
    int n = 20;
    scanf("%lf %lf", &a1, &b1);
    scanf("%lf %lf", &a2, &b2);
    scanf("%lf %lf", &a3, &b3);
    p = fsin;
    c = integral(p, a1, b1, n);
    printf("the integral of sin(x) is: %f\n", c);
    p = fcos;
    c = integral(p, a2, b2, n);
    printf("the integral of cos(x) is: %f\n", c);
    p = fexp;
    c = integral(p, a3, b3, n);
    printf("the integral of exp(x) is: %f\n", c);
    return 0;
}
```



### 36

**用指针对字符串排序后输出**

在主函数中输入 10 个不等长的字符串保存到用指针数组中。用另一函数对它们按从小到大的顺序排序。然后在主函数输出这 10 个已排好序的字符串。

```C
#include <stdio.h>
#include <string.h>

void sort (char *s[]) {
    int i, j;
    char *temp;
    for (i = 0; i < 9; i++)
        for (j = 0; j < 9 - i; j++)
            if (strcmp(*(s + j), *(s + j + 1)) > 0) {
                temp = *(s + j);
                *(s + j) = *(s + j + 1);
                *(s + j + 1) = temp;
            }
}

int main () {
    int i;
    char *p[10], str[10][20];
    for (i = 0; i < 10; i++)
        p[i] = str[i];
    for (i = 0; i < 10; i++)
        scanf("%s", p[i]);
    sort(p);
    for (i = 0; i < 10; i++)
        printf("%s\n", p[i]);
    return 0;
}
```



### 37

**对字符串排序后输出**

在主函数中输入 10 个等长的字符串。用另一函数对它们按从小到大的顺序排序。然后在主函数输出这 10 个已排好序的字符串。

```C
#include <stdio.h>
#include <string.h>

void sort (char s[10][6]) {
    int i, j;
    char *p, temp[10];
    p = temp;
    for (i = 0; i < 9; i++)
        for (j = 0; j < 9 - i; j++)
            if (strcmp(s[j], s[j + 1]) > 0) {
                strcpy(p, s[j]);
                strcpy(s[j], s[j + 1]);
                strcpy(s[j + 1], p);
            }
}

int main () {
    int i;
    char str[10][6];
    for (i = 0; i < 10; i++)
        scanf("%s", str[i]);
    sort(str);
    for (i = 0; i < 10; i++)
        printf("%s\n", str[i]);
    return 0;
}
```



### 38

**调整矩阵元素位置**

将一个 5x5 的矩阵中最大的元素放在中心，4 个角分别放 4 个最小的元素(顺序为从左到右，从上到下依次从小到大存放)，写一函数实现。

```C
#include <stdio.h>

void change (int *p) {
    int i, j, temp;
    int *pmax, *pmin;
    pmax = p;
    pmin = p;
    for (i = 0; i < 5; i++)
        for (j = 0; j < 5; j++) {
            if (*pmax < *(p + 5 * i + j))
                pmax = p + 5 * i + j;
            if (*pmin > *(p + 5 * i + j))
                pmin = p + 5 * i + j;
        }
    temp = *(p + 12);
    *(p + 12) = *pmax;
    *pmax = temp;
    temp = *p;
    *p = *pmin;
    *pmin = temp;
    pmin = p + 1;
    for (i = 0; i < 5; i++)
        for (j = 0; j < 5; j++)
            if (((p + 5 * i + j) != p) && (*pmin > *(p + 5 * i + j)))
                pmin = p + 5 * i + j;
    temp = *pmin;
    *pmin = *(p + 4);
    *(p + 4) = temp;
    pmin = p + 1;
    for (i = 0; i < 5; i++)
        for (j = 0; j < 5; j++)
            if (((p + 5 * i + j) != (p + 4)) && ((p + 5 * i + j) != p) && (*pmin > *(p + 5 * i + j)))
                pmin = p + 5 * i + j;
    temp = *pmin;
    *pmin = *(p + 20);
    *(p + 20) = temp;
    pmin = p + 1;
    for (i = 0; i < 5; i++)
        for (j = 0; j < 5; j++)
            if (((p + 5 * i + j) != p) && ((p + 5 * i + j) != (p + 4)) && ((p + 5 * i + j) != (p + 20)) && (*pmin > *(p + 5 * i + j)))
                pmin = p + 5 * i + j;
    temp = *pmin;
    *pmin = *(p + 24);
    *(p + 24) = temp;
}

int main () {
    int a[5][5], *p, i, j;
    for (i = 0; i < 5; i++)
        for (j = 0; j < 5; j++)
            scanf("%d", &a[i][j]);
    p = &a[0][0];
    change(p);
    for (i = 0; i < 5; i++) {
        for (j = 0; j < 5; j++)
            printf("%d ", a[i][j]);
        printf("\n");
    }
    return 0;
}
```



### 39

**整型矩阵转置**

写一个函数，将一个 3x3 的整型矩阵转置。

```C
#include <stdio.h>

void move (int *pointer) {
    int i, j, t;
    for (i = 0; i < 3; i++)
        for (j = i; j < 3; j++) {
            t = *(pointer + 3 * i + j);
            *(pointer + 3 * i + j) = *(pointer + 3 * j + i);
            *(pointer + 3 * j + i) = t;
        }
}

int main () {
    int a[3][3], *p, i;
    for (i = 0; i < 3; i++)
        scanf("%d %d %d", &a[i][0], &a[i][1], &a[i][2]);
    p = &a[0][0];
    move(p);
    for (i = 0; i < 3; i++)
        printf("%d %d %d\n", a[i][0], a[i][1], a[i][2]);
    return 0;
}
```



### 40

**报数退出**

由 n 个人围成一圈，顺序排号。从第一个人开始报数(从 1 到 3 报数)，凡报到 3 的人退出圈子，问最后留下的是原来第几号的那位。

```C
#include <stdio.h>

int main () {
    int i, k, m, n, num[50], *p;
    scanf("%d", &n);
    p = num;
    for (i = 0; i < n; i++)
        *(p + i) = i + 1;
    i = 0;
    k = 0;
    m = 0;
    while (m < n - 1) {
        if (*(p + i) != 0)
            k++;
        if (k == 3) {
            *(p + i) = 0;
            k = 0;
            m++;
        }
        i++;
        if (i == n)
            i = 0;
    }
    while (*p == 0)
        p++;
    printf("%d\n", *p);
    return 0;
}
```



### 41

**移动数组中的多个数字**

输入由 n 个整数组成的数组，使用指针变量将其中前面各数顺序向后移 m 个位置，最后 m 个数变成最前面 m 个数。

```C
#include <stdio.h>

void move (int array[20], int n, int m) {
    int *p, array_end;
    array_end = *(array + n - 1);
    for (p = array + n - 1; p > array; p--)
        *p = *(p - 1);
    *array = array_end;
    m--;
    if (m > 0)
        move(array, n, m);
}

int main () {
    int number[20], n, m, i;
    scanf("%d", &n);
    for (i = 0; i < n; i++)
        scanf("%d", &number[i]);
    scanf("%d", &m);
    move(number, n, m);
    for (i = 0; i < n; i++)
        printf("%d ", number[i]);
    printf("\n");
    return 0;
}
```



### 42

**移动数组中的数字**

输入由 10 个整数组成的数组，使用指针变量将其中最小的数与第 1 个数对换，将最大的数与最后一个数对换。

```C
#include <stdio.h>

void input(int *number) {
    int i;
    for (i = 0; i < 10; i++)
        scanf("%d", &number[i]);
}

void max_min_value (int *number) {
    int  *max, *min, *p, temp;
    max = min = number;
    for (p = number + 1; p < number + 10; p++)
        if (*p > *max)
            max = p;
        else if (*p < *min)
            min = p;
    temp = number[0];
    number[0] = *min;
    *min = temp;
    if (max == number)
        max = min;
    temp = number[9];
    number[9] = *max;
    *max = temp;
}

void output (int *number) {
    int *p;
    for (p = number; p < number + 10; p++)
        printf("%d ", *p);
    printf("\n");
}

int main () {
    int number[10];
    input(number);
    max_min_value(number);
    output(number);
    return 0;
}
```



### 43

**使用指针对字符串进行排序**

使用指针变量对输入的 3 个字符串进行从小到大排序。

```C
#include <stdio.h>
#include <string.h>

void swap (char *p1, char *p2) {
    char p[20];
    strcpy(p, p1);
    strcpy(p1, p2);
    strcpy(p2, p);
}

int main () {
    char str1[20], str2[20], str3[30];
    gets(str1);
    gets(str2);
    gets(str3);
    if (strcmp(str1, str2) > 0)
        swap(str1, str2);
    if (strcmp(str1, str3) > 0)
        swap(str1, str3);
    if (strcmp(str2, str3) > 0)
        swap(str2, str3);
    printf("%s\n%s\n%s\n", str1, str2, str3);
    return 0;
}
```



### 44

**输出地址**

**编程实现：** 预置代码中定义了两个变量，请以十六进制形式输出它们的地址。

```C
#include <stdio.h>
int main () {
    int a = 100;
    char str[20] = "alphacoding.cn";
    printf("%#X %#X", &a, str);
    return 0;
}
```



### 45

根据结束时间和用时, 推算开始时间.

**注意:** 本题采用 24 小时制。即时间范围在`0:00:00.00 ~ 23:59:59.99`之间。

**函数定义**

```
void time_sub (int *start_hour, int *start_minute, double *start_second, int end_hour, int end_minute, double end_second, double duration);
```

**参数说明**

- `start_hour`, 整型指针, 表示开始时间的小时
- `start_minute`, 整型指针, 表示开始时间的分钟
- `start_second`, 双精度浮点数指针, 表示开始时间的秒数
- `end_hour`, 整型, 表示结束时间的小时
- `end_minute`, 整型, 表示结束时间的分钟
- `end_second`, 双精度浮点数, 表示结束时间的秒数
- `duration`, 双精度浮点数, 表示用时秒数, 其值在`0.00 ~ 86400.00`之间

**示例1**

参数

```
end_hour = 8
end_minute = 26
end_second = 0.2
duration = 15.2
```

输出

```
start_hour: 8
start_minute: 25
start_second: 45.00
```

**示例2**

参数

```
end_hour = 0
end_minute = 0
end_second = 0.4
duration = 0.53
```

输出

```
start_hour: 23
start_minute: 59
start_second: 59.87
```

```C
#include <stdio.h>

void time_sub (int *start_hour, int *start_minute, double *start_second, int end_hour, int end_minute, double end_second, double duration) {
    // TODO 请在此处编写代码，完成题目要求
    double start_time, end_time;
	end_time = end_hour * 60 * 60 + end_minute * 60 + end_second;
	start_time = end_time - duration;
	if (start_time < 0) {
		start_time = 24 * 60 * 60 + start_time;
	}
	*start_hour = (int)start_time / 3600;
	*start_minute = (int)start_time % 3600 / 60;
	*start_second = start_time - *start_hour * 3600 - *start_minute * 60;
}

int main () {
    int start_hour, start_minute, end_hour = 8, end_minute = 26;
    double start_second, end_second = 0.2, duration = 15.2;
    time_sub(&start_hour, &start_minute, &start_second, end_hour, end_minute, end_second, duration);
    printf("start_hour: %d\nstart_minute: %d\nstart_second: %.2f\n", start_hour, start_minute, start_second);
    return 0;
}
```



### 46

统计一个长度为 3 的字符串在另一个字符串中出现的次数. 如, 字符串"asd"在字符串"asdasasdfgasdaszx67asdmklo"中出现 4 次.

**函数定义**

```
int count_substr (char *str, char *substr);
```

**参数说明**

- `str`, 字符串指针, 表示待查找的字符串
- `substr`, 字符串指针, 表示待统计的字符串

**返回值说明**

函数返回`substr`在`str`中出现的次数; 其类型为 int.

```C
#include <stdio.h>

// 函数用于统计子字符串在主字符串中的出现次数
int count_substr(char *str, char *substr) {
    int count = 0;
    while (*str) {
        // 判断当前位置开始的三个字符是否与子字符串相同
        if (*str == substr[0] && *(str + 1) == substr[1] && *(str + 2) == substr[2]) {
            count++; // 相同则增加计数
        }
        str++; // 移动到主字符串的下一个位置
    }
    return count; // 返回统计结果
}

int main() {
    char str[100];  // 假设输入的字符串长度不超过 99
    char substr[4]; // 假设输入的子字符串长度为 3，再加上字符串结尾的空字符 '\0'
    
    fgets(str, sizeof(str), stdin); // 从标准输入获取主字符串
    
    fgets(substr, sizeof(substr), stdin); // 从标准输入获取要统计的子字符串
    
    int result = count_substr(str, substr); // 调用统计函数
    
    printf("%d", result);
    
    return 0;
}
```



### 47

找出有`n`个元素的指针数组中最长的字符串的长度, 并将其返回.

**函数定义**

```
int max_len (int n, char *str[]);
```

**参数说明**

- `n`, 整型, 表示数组长度
- `str`, 字符串指针, 表示指针数组

**返回值说明**

函数返回最长的字符串长度; 其类型为 int.

**示例 1**

参数

```
n = 4
str = {"blue", "yellow", "red", "green"}
```

返回

```
6
```

```C
#include <stdio.h>
#include <string.h>

int max_len (char *str[], int n) {
    // TODO 请在此处编写代码，完成题目要求
	int max_len = strlen(str[0]);
    for (int i = 0; i < n; i++) {
		int len = strlen(str[i]);
		if (len > max_len) {
			max_len = len;
		} 
	}
    return max_len;
}

int main () {
    int n = 4, ret;
    char *str[] = {"blue", "yellow", "red", "green"};
    ret = max_len(str, n);
    printf("%d\n", ret);
    return 0;
}
```



### 48

**查找星期	**

设每个星期都有一个对应的序号，其序号和星期的对照表如下：

| 序号 | 星期      |
| :--- | :-------- |
| 0    | Sunday    |
| 1    | Monday    |
| 2    | Tuesday   |
| 3    | Wednesday |
| 4    | Thursday  |
| 5    | Friday    |
| 6    | Saturday  |

**编程实现：** 请完善函数`getindex`。`getindex`函数的功能是判断指定的字符串是否是星期，如果是，则返回其对应的序号；如果不是，则返回`-1`。

注意：请不要修改 main 函数中的代码。

**参数说明**

- `str`, 字符串指针, 表示需要判断的字符串。

**示例 1**

参数

```
*str = "Tuesday"
```

输出

```
2
```

```C
#include <stdio.h>

int getindex (char *str) {
	if (str == "Sunday") return 0;
	else if (str == "Monday") return 1;
	else if (str == "Tuesday") return 2;
	else if (str == "Wednesday") return 3;
	else if (str == "Thursday") return 4;
	else if (str == "Friday") return 5;
	else if (str == "Saturday") return 6;
	else return -1;
}

int main () {
    int n;
    char *str = "Tuesday";
    n = getindex(str);
    printf("%d\n", n);
    return 0;
}
```



### 49

**计算两数的和与差**

**编程实现：** 请完善函数`sum_diff`。`sum_diff`函数的功能是计算两个实数`op1`和`op2`的和`*psum`与差`*pdiff`（op1 - op2 的值）。

注意：请不要修改 main 函数中的代码。

**参数说明**

- `op1`, 浮点数, 表示待操作数一。
- `op2`, 浮点数, 表示待操作数二。
- `psum`, 浮点数指针, 表示存储两数和的指针。
- `pdiff`, 浮点数指针, 表示存储两数差的指针。

```C
#include <stdio.h>

void sum_diff (float op1, float op2, float *psum, float *pdiff) {
    // TODO 请在此处编写代码，完成题目要求
    *psum = op1 + op2;
    *pdiff = op1 - op2;
}

int main () {
    float op1 = 4, op2 = 6, psum, pdiff;
    sum_diff (op1, op2, &psum, &pdiff);
    printf("sum: %f\ndiff: %f", psum, pdiff);
    return 0;
}
```



### 50

**利用指针找最大值**

**编程实现：** 请完善函数`find_max`。`find_max`函数的功能是找出两个指针所指向的整数中的最大值，并将其保存在一个指针中。

注意：请不要修改 main 函数中的代码。

**参数说明	**

- `px`,整数指针，表示整数一。
- `py`,整数指针，表示整数二。
- `pmax`，整数指针，表示需要保存的指针。

```C
#include <stdio.h>

void find_max (int *px, int *py, int *pmax) {
    // 请在此处编写代码，完成题目要求
    if (*px > *py) {
        *pmax = *px;
    } else {
        *pmax = *py;
    }
}

int main () {
    int x = 11, y = 22, max;
    find_max (&x, &y, &max);
    printf("%d\n", max);
    return 0;
}
```



### 51

**查找子串**

在字符串中查找子串, 如果能找到, 返回子串首字符在母串中的位置(该位置从 0 开始); 若没有找到子串, 返回 -1。

**函数定义**

```
int search (char *str, char *substr);
```

**参数说明**

- `str`, 字符串指针, 表示待查找的字符串
- `substr`, 字符串指针, 表示子串

**返回值说明**

函数返回子串首字符在母串中的位置; 其类型为`int`.

**示例 1**

参数

```
str = "abcdefghijklmn"
substr = "def"
```

返回

```
3
```

```C
#include <stdio.h>
#include <string.h>

char temp_str[30];

void read_str_unit (char *str, char *temp_str, int idx, int len) {
	int index = 0;
	for (index = 0; index < len; index++) {
		temp_str[index] = str[idx+index];
	}
	temp_str[index] = '\0';
}

int search (char *str, char *substr) {
    // TODO 请在此处编写代码，完成题目要求
    int idx = 0;
	int len1 = strlen(str);
	int len2 = strlen(substr);
	if (len1 < len2) {
		printf("error\n");
		return -1;
	}
	while (1) {
		read_str_unit(str, temp_str, idx, len2);
		if (strcmp(substr, temp_str) == 0) break;
		idx++;
		if (idx >= len1) return -1;
	}
    return idx;
}

int main () {
	char *str = "abcdefghijklmn", *substr = "def";
	int i = 0;
	i = search(str, substr);
	printf("%d\n", i);
	return 0;
}
```



### 52

**拆分实数的整数与小数部分**

**编程实现：** 请完善函数`splitfloat`。`splitfloat`函数的功能是拆分一个给定实数`number`的整数与小数部分，并将它们存储到指针变量中。

注意：请不要修改 main 函数中的代码。

**参数说明**

- `number`，双精度浮点型，表示需要拆分的整数。
- `intpart`，整型，表示拆分出来的整数部分。
- `fracpart`，双精度浮点型，表示拆分出来的小数部分。

```C
#include <stdio.h>

void splitfloat (double number, int *intpart, double *fracpart) {
    *intpart = (int)number;
	*fracpart = number - *intpart;
}

int main () {
    double number = 123.456;
    int intpart;
    double fracpart;
    splitfloat (number, &intpart, &fracpart);
    printf("intpart: %d\nfracpart: %g", intpart, fracpart);
    return 0;
}
```



### 53

**移动字母**

将指定字符串的前三个字符移到最后.

**函数定义**

```
void shift (char *str);
```

**参数说明**

- `str`, 字符串指针, 表示需要进行移动的字符串

```C
#include <stdio.h>

void shift (char *str) {
	// TODO 请在此处编写代码，完成题目要求
    char string[3];
	int i, j;
	for (i = 0; i < 3; i++)
		string[i] = str[i];
	for (i = 3; str[i] != '\0'; i++)
		str[i-3] = str[i];
	for (j = i - 3, i = 0; i < 3; i++)
		str[j++] = string[i];
}

int main () {
    char str[100] = "I am a student. I like programming.";
    shift(str);
    printf("%s", str);
    return 0;
}
```



### 54

**字符串部分复制**

将传入的字符串`str1`中从第`index`个字符开始的全部字符复制到字符串`str2`中。

**注意:** index 必须小于 字符串的长度。复制时, 包含第`index`个字符.

**函数定义**

```
void strmcpy (char *str1, int index, char *str2);
```

**参数说明**

- `str1`, 字符串指针, 表示被复制的字符串
- `index`, 整型, 表示指定的位置
- `str2`, 字符串指针, 表示完成复制的字符串

```C
#include <stdio.h>
#include <string.h>

void strmcpy (char *str1, int index, char *str2) {
    // TODO 请在此处编写代码，完成题目要求
	int i, j, len;
	strcpy(str2, str1);
	len = strlen(str2);
	for(i = index - 1; i > 0; i--){
		for(j = i; j < len; j++){
			*(str2 + j - 1) = *(str2 + j);
		}
	}
	*(str2 + len - index + 1)='\0';
}

int main () {
    char str1[100] = "I am a student. I like programming.", str2[100];
    int index = 10;
    strmcpy(str1, index, str2);
    printf("%s", str2);
    return 0;
}
```



### 55

**月份英文名**

**编程实现：** 请完善函数`*getmonth`。`*getmonth`函数的功能是返回月份`month`的英文名称。如果传入的参数`month`不是一个代表月份的数字，则返回`NULL`。

注意：请不要修改 main 函数中的代码。从一月到十二月的英文名分别是`January`,`February`,`March`,`April`,`May`,`June`,`July`,`August`,`September`,`October`,`November`,`December`

**参数说明**

- `month`，整型，表示传入的月份，其值为`1 ~ 12`的整数。

```C
#include <stdio.h>

char *getmonth (int month) {
    switch (month) {
		case 1:
			return "January";
		case 2:
			return "February";
		case 3:
			return "March";
		case 4:
			return "April";
		case 5:
			return "May";
		case 6:
			return "June";
		case 7:
			return "July";
		case 8:
			return "August";
		case 9:
			return "September";
		case 10:
			return "October";
		case 11:
			return "November";
		case 12:
			return "December";
		default:
			return NULL;
	}
    return NULL;
}

int main () {
    char *month_name;
    int month = 5;
    month_name = getmonth (month);
    printf("%s", month_name);
    return 0;
}
```





### 56

**字符排队**

将给定字符串中的字符，按照 ASCII 码顺序从小到大排序后输出。

**输入格式**

输入是一个以回车结束的非空字符串（少于80个字符）。

> 注：不能使用内置函数排序

**输出格式**

输出排序后的结果字符串。

```C
#include <stdio.h>

int main() {
    
    char s[200] = {};
    scanf("%s", s);
    
    // 求长度
    int len = 0;
    char *p = s;
    while(*(p++)) len++;
    
    for (int i=0; i<len; i++) {
     	for (int j=1; j<len - i; j++) {
			if (s[j] < s[j-1]) {
             	char c = s[j];
                s[j] = s[j-1];
                s[j-1] = c;
            }
        }
    }
	
    printf("%s\n%d", s, len);
 
    return 0;
}
```



### 57

**提取字符串中的数字组成整数**

**编程实现：** 将一个字符串中的所有数字字符(‘0’…‘9’)提取出来, 将其转换为一个整数, 并将转换后的整数输出.

```C
#include <stdio.h>
#include <string.h>

int main() {
	char s[200] = {};
	gets(s);
	char *p = s;
	int s_len = strlen(s);
	char a[30] = {};
	long a_len = 0;
	for (int i = 0; i < s_len; i++) {
		char c = s[i];
		if (c >= '0' && c <= '9') {
			a[a_len] = c;
			a_len++;
		}
	}
	long l = 0;
	long mul = 1;
	for (int i = a_len - 1; i >= 0; i--) {
		l += ((a[i] -= '0') * mul);
		mul *= 10;
	}
	printf("%ld\n", l);
	return 0;
}
```



### 58

**删除重复字符**

将一个字符串去掉重复的字符后, 按照字符 ASCII 码顺序从小到大排序后输出.

**输入格式**

输入是一个以回车结束的非空字符串（少于80个字符）。

**输出格式**

输出去重排序后的结果字符串。

```C
#include <stdio.h>
#include <string.h>
#define SIZE 81

void delete_repeat(char *str);
void bubble_sort(char *str);

int main()
{
    char str[SIZE];
    gets(str);
    delete_repeat(str);
    bubble_sort(str);
    puts(str);
    return 0;
}
//删除重复字符
void delete_repeat(char *str)
{
    for(int i=1;str[i]!='\0';i++){
        for(int j=0;j<i;j++) 
        {
            if(str[i]==str[j])
            {
                for(int k=i;k<strlen(str)-1;k++)
                {
                    str[k]=str[k+1];
                }
                str[strlen(str)-1]='\0';
                i--;
            }
        }
    }
}

//冒泡排序
void bubble_sort(char *str)
{
    int swap;
    char temp;
    int k=strlen(str);
    do{
        swap =0;
        for(int i=0;i<k-1;i++){
            if(str[i]>str[i+1])
            {
                swap=1;
                temp=str[i];
                str[i]=str[i+1];
                str[i+1]=temp;
            }
        }
        k--;
    }while(k>0&&swap);
}
```



### 59

**字符串相减**

将给定的两个字符串相减. 即从第一个字符串中把第二个字符串中所包含的字符全删掉, 并将剩下的字符输出.

**输入格式**

输入在 2 行中先后给出字符串 A 和 B。两字符串的长度都不超过 1000, 并且保证每个字符串都是由可见的 ASCII 码和空白字符组成，最后以换行符结束。

**输出格式**

在一行中打印出A−B的结果字符串。

```C
#include <stdio.h>

int main()
{
    char s1[1024] = {};
    char s2[1024] = {};

    // 0=not in s2; 1=in s2;
    char mark[256] = {};
    gets(s1);
    gets(s2);

    char *p = s2;
    while (*p)
    {
        mark[*p] = 1;
        p++;
    }

    p = s1;
    while (*p)
    {
        char c = *p;
        if (!mark[c])
        {
            printf("%c", c);
        }
        p++;
    }
}
```



### 60

**翻转单词顺序**

编写程序，输入一行字符串，翻转该字符串，翻转时单词中的字符顺序不变。例如，如果字符串为"Hello World"，则翻转后为 “World Hello”。单词间以一个或多个空格分隔。注意，字符串开头和结尾都可能有多个空格。

**输入格式**

输入一行字符串，除了空格外，标点符号和普通字母一样处理。
字符总数不会超过 500 个，单词数不会超过 50，每个单词的长度不会超过 30。

**输出格式**

输出包括多行，每行对应输入的一行，为翻转后的字符串。

```C

#include<stdio.h>
#include<string.h>
void reverse(char *s, int begin, int end)
{
	char temp;
	while(end > begin) {
		temp = s[begin];
		s[begin] = s[end];
		s[end] = temp;
		begin++;
		end--;
	}
	
}
int main()
{
	int a, b;
	a = b = 0;
	char s[100];
	gets(s);
	reverse(s, 0, strlen(s)-1);
	int i = 0;
	while(i < strlen(s)) {
		//跳过空格 
		while(s[i] == ' ' && i < strlen(s)) {
			i++;
		}
		a = i;
		//跳过非空格字符 
		while(s[i] != ' ' && i < strlen(s)) {
			i++;
		}
		b = i-1;
		//a~b为一个单词的下标的区间 
		reverse(s,a, b);
	}
	puts(s);
	return 0;
}
```



### 61

**鸡兔同笼**

若笼子里只有鸡和兔子, 并且共有`35`个头,`94`只脚, 则笼子里鸡有`23`只, 兔子有`12`只.

**请用程序实现:** 定义函数`chickenrabbit`并接收四个`int`型参数`*chicken, *rabbit, head, foot,`. 其中`head`表示头的数量,`foot`表示脚的数量,`*chicken`是表示鸡的数量的指针,`*rabbit`是表示兔子的数量的指针; 在函数内计算鸡和兔子的数量, 若问题有解, 则将鸡和兔子的数量保存到`chicken`和`rabbit`所指示的变量中, 且函数`chickenrabbit`的返回值为`1`; 若问题无解, 则不改变`chicken`和`rabbit`所指示的变量, 且函数`chickenrabbit`的返回值为 `0`.

**注意:** 函数`chickenrabbit`返回值的类型为`int`型.

```C
#include <stdio.h>

/* 函数声明 */
int chickenrabbit (int *chicken, int *rabbit, int head, int foot);

int main () {
    int chicken, rabbit, head = 35, foot = 94;
    if (chickenrabbit (&chicken, &rabbit, head, foot)) {
        printf("%d %d\n", chicken, rabbit);
    } else {
        printf("None\n");
    }
    return 0;
}

/* 请在此处完成你的程序 */
int chickenrabbit (int *chicken, int *rabbit, int head, int foot) {
    int flag;
    flag = foot - head * 2;
    if (flag >= 0 && flag % 2 == 0) {
        int r = flag / 2;
        int c = head - r;
        *chicken = c;
        *rabbit = r;
        return 1;
    } else {
        return 0;
    }
}
```



### 62

**用指针求平均成绩**

**编程实现：** 请完善函数`average`。`average`函数的功能是计算成绩数组中的平均成绩，并将平均成绩返回。

注意：请不要修改 main 函数中的代码。

**参数说明**

- `n`，整型，表示成绩数组的长度。
- `score`,整型指针，表示保存成绩的数组。

```C
#include <stdio.h>

double average (int n, int *score) {
    double ave = 0;
    for (int i = 0; i < n; i++) {
        ave = ave + score[i];
    }
    ave = ave / n;
    return ave;
}

int main () {
    int n = 10, score[10] = {88, 99, 76, 78, 89, 93, 95, 86, 92, 85};
    double ave_score;
    ave_score = average(n, score);
    printf("%f\n", ave_score);
    return 0;
}
```



### 63

**用指针实现字符串复制**

**编程实现：** 请完善函数`StringCopy`。`StringCopy`函数的功能是复制字符串，并将复制的字符串输出。

注意：请不要修改 main 函数中的代码。

**参数说明**

- `p`，字符指针，表示被复制的字符串，其长度为 20。
- `q`，字符指针，表示复制的字符串，其长度为 20。

```C
#include <stdio.h>

void StringCopy (char *p, char *q) {
    for (; *p != '\0'; q++, p++) {
        *q = *p;
    }
    *q = '\0';
}

int main () {
    char p[20] = "qwertyuiop", q[20];
    StringCopy(p, q);
    printf("%s\n", q);
    return 0;
}
```



### 64

**用指针对数组元素排序**

**编程实现：** 请完善函数`sort`。`sort`函数的功能是使用指针对数组元素进行由小到大排序。

注意：请不要修改 main 函数中的代码。

**参数说明**

- `n`，整型，表示数组长度。
- `p`，整型指针，表示需要排序的数组。

```C
#include <stdio.h>

void sort(int n, int *p) {
	int i,j,t;
	for(i=0; i<n-1; i++)
		for(j=0; j<n-1-i; j++)
			if(p[j]>p[j+1]) {
				t=p[j];
				p[j]=p[j+1];
				p[j+1]=t;
			}
}

int main() {
	int arr[10],i;
	for(i=0; i<10; i++)
		scanf("%d",&arr[i]);
	sort(10, arr);
	printf("排序后：");
	for(i=0; i<10; i++)
		printf("%d ",arr[i]);
	return 0;
}
```



### 65

**用指针对两个数进行交换**

**编程实现：** 请完善函数`exchange`。`exchange`函数的功能是用指针对两个整数进行交换。

注意：请不要修改 main 函数中的代码。

**参数说明**

- `p`，整型指针，存储待交换的数字一。
- `q`，整型指针，存储待交换的数字二。

```C
#include <stdio.h>

void exchange (int *p,int *q) {
    int t;
    t=*p;
    *p=*q;
    *q=t;
}

int main() {
    int a,b;
    scanf("%d %d",&a,&b);
    exchange(&a,&b);
    printf("交换后：%d %d\n",a,b);
    return 0;
}
```



### 66

**用指针统计字符数量**

**编程实现：** 预置代码定义了一个字符数组，存放 100 个元素，请使用 gets 函数获取一个字符串，然后使用指针统计字符串中大写字母、小写字母、空格及数字的个数。

> 注：输出时，请按照大写字母、小写字母、空格、数字的顺序输出。

```C
#include <stdio.h>
int main()
{
	char str[100],*p;
	int k[4]={0};
	p=str;
	gets(p);
	for(;*p!='\0';p++)
	{
		if(*p>='A' && *p<='Z')
			k[0]++;
		else if(*p>='a' && *p<='z')
			k[1]++;
		else if(*p==' ')
			k[2]++;
		else if(*p>='0' && *p<='9')
			k[3]++;
	}
	printf("大写字母：%d\n小写字母：%d\n空格：%d\n数字：%d\n",k[0],k[1],k[2],k[3]);
	return 0;
}
```



### 67

**用指针求字符串长度**

定义一个字符数组，存放100个元素，使用gets函数输入一个字符串，然后用字符指针实现求字符串长度。

```C
#include <stdio.h>
int main()
{
	char str[100],*p;
	int k=0;
	p=str;
	gets(p);
	for(;*p!='\0';p++)
		k++;
	printf("%d\n",k);
	return 0;
}
```



### 68

**使用指针进行排序**

**编程实现：** 请完善函数`swap`和`main`。`swap`函数的功能是实现两个整数的交换功能。然后在主函数中调用`swap`函数实现对输入的 3 个整数进行从小到大排序。

**参数说明**

- `p1`，整型指针，存储待交换的数字一。
- `p2`，整型指针，存储待交换的数字二。

```C
#include <stdio.h>

void swap (int *p1, int *p2) {
    int p;
    p = *p1;
    *p1 = *p2;
    *p2 = p;
}

int main () {
	   int n1, n2, n3, *p1, *p2, *p3;
    scanf("%d %d %d", &n1, &n2, &n3);
    p1 = &n1;
    p2 = &n2;
    p3 = &n3;
    if (n1 > n2)
        swap(p1, p2);
    if (n1 > n3)
        swap(p1, p3);
    if (n2 > n3)
        swap(p2, p3);
    printf("%d %d %d\n", *p1, *p2, *p3);
    return 0;
}
```

