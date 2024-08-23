# Alpha 栈和队列

## 入栈出栈操作合法性判断

假设以 0 和 1 分别表示出栈和入栈操作。栈的初始为空，编写函数，判断入栈和出栈操作的合法性，当操作序列合法时，主函数输出 True，否则，输出 False。

**示例一：**

操作序列

```
01001010
```

输出

```
False
```

**示例二：**

操作序列

```
11001010
```

输出

```
True
```



#### 题目

```c
//判断栈操作合法性
#include <stdio.h>
#include <stdlib.h>

//判断入栈出栈操作序列是否是合法序列。如是，返回1，否则返回0
//输入参数：操作序列
int Judge(char ch[])
{
	
}

int main()
{
	char ch[] = "11001010";
	int i = 0;

	if(Judge(ch))
		printf("True");
	else
		printf("False");

    return 0;
}

```

#### 答案

> 两个需要注意的地方吧，第一是`ch[i] != '\0'`，别写成`'\n'`。第二是`if (ch[i] == '1')`，注意是字符，不是数字。

```c
#include <stdio.h>
#include <stdlib.h>

// 判断入栈出栈操作序列是否是合法序列。如是，返回1，否则返回0
// 输入参数：操作序列
int Judge(char ch[])
{
    int stack[100] = { 0 };  // 栈
    int top = -1;         // 栈顶指针

    for (int i = 0; ch[i] != '\0'; i++) {
        if (ch[i] == '1') {
            // 入栈操作
            stack[++top] = 1;
        }
        else if (ch[i] == '0') {
            // 出栈操作
            if (top == -1) {
                // 栈为空
                return 0;
            }
            else {
                // 匹配成功，出栈
                top--;
            }
        }
        else {
            // 非法字符
            return 0;
        }
    }

    // 操作序列遍历完毕，如果栈为空则序列合法，否则不合法
    // 错误的，理解错了，最后不一定的得全拿出来
    //return top == -1;
    return 1;
}

int main()
{
    char ch[] = "11011010";

    if (Judge(ch))
        printf("True\n");
    else
        printf("False\n");

    return 0;
}
```

### 标准答案（不太好）

```c
 //判断入栈出栈操作序列是否是合法序列。如是，返回1，否则返回0
//输入参数：操作序列
int Judge(char ch[])
{
	int i, top;
	i = 0;
	top = 0;

	while (ch[i] != '\0')
	{
		switch (ch[i])
		{
		case '1':
			top++;
			break;
		case '0':
			top--;
			if (top < 0) {
				return 0;
			}
			break;
		}
		i++;
	}
	return 1;
}
```





## 判断回文字符串

1、试写一个算法，实现判断输入的字符串是否为回文字符串的功能。如果是回文字符串则输出`该字符串是回文字符串！`，如果不是则输出`该字符串不是回文字符串！`。
2、使用栈的数据结构来实现判断。
3、回文字符串是指正向和反向读都一样的字符串。例如，“radar”、"level"都是回文字符串，而"hello"不是。
4、程序应该能够处理任意长度的字符串。

**示例 1：**

输入：

```
abba
```

输出：

```
该字符串是回文字符串！
```

**示例 2：**

输入：

```
good
```

输出：

```
该字符串不是回文字符串！
```

### 注意事项

1、输入的字符串长度不超过栈的最大容量。
2、程序中使用了动态内存分配来创建栈，确保释放内存以防止内存泄漏。



#### 题目

```c
#include <stdio.h>
#include <malloc.h>	  //分配内存时应用
#define StackSize 100 //设置栈的最大内存空间

typedef struct {
    //定义栈结构体
    char data[StackSize];
    int top;
} SeqStack;

int Push_Stack (SeqStack *s, char x);
int Pop_Stack (SeqStack *s, char *x);

int IsEmpty (SeqStack *s) {
    //判断栈是否为空
    
}

int IsHuiwen () {
    //判断回文数
    
}

int Push_Stack (SeqStack *s, char chr) {
    //进行入栈操作
    
}

int Pop_Stack (SeqStack *s, char *chr) {
    //取出栈顶元素
    
}

int main () {
    printf("请输入字符串序列：");
    if (IsHuiwen())
        printf("\n该字符串是回文字符串！\n");
    else
        printf("\n该字符串不是回文字符串！\n");
}
```

#### 答案

> 这段代码使用了栈的数据结构来判断输入的字符串是否为回文字符串。首先，将输入的字符串逐个字符入栈，然后再逐个字符出栈并与原字符串进行比较，如果每次出栈的字符与原字符串对应位置的字符相等，则继续比较下一个字符，否则返回不是回文字符串。如果比较完成后没有返回，则说明是回文字符串。
>
> 注意，在本代码中，我添加了对栈满和栈空的检查，以及对输入字符串长度的限制。
>
> 还需要注意的是，在使用动态内存分配函数`malloc`和释放内存函数`free`时，需要包含头文件`stdlib.h`。
>
> 还有就是题目中的`main()`不是很完善，我稍稍改了改。在输入字符串时使用了`fgets`函数，可以处理任意长度的字符串，并且去除了末尾的换行符。（`string.h`）
>
> 一定要自己完善一下`main()`，否则过不了检查。

> 考试别用自己的代码，因为标准答案不需要改`main()`也能通过检查。因为标准答案都把一系列操作集成到`IsHuiwen()`里面了。

```c
#include <stdio.h>
#include <stdlib.h>	  // 使用动态内存分配函数
#include <string.h>   // 使用字符串处理函数

#define StackSize 100 // 设置栈的最大内存空间

typedef struct {
    // 定义栈结构体
    char data[StackSize];
    int top;
} SeqStack;

int IsEmpty(SeqStack* s) {
    // 判断栈是否为空
    return s->top == -1;
}

int Push(SeqStack* s, char chr) {
    // 进行入栈操作
    if (s->top == StackSize - 1) {
        printf("栈已满，无法入栈！\n");
        return 0;
    }

    s->top++;
    s->data[s->top] = chr;
    return 1;
}

int Pop(SeqStack* s, char* chr) {
    // 取出栈顶元素
    if (IsEmpty(s)) {
        printf("栈为空，无法出栈！\n");
        return 0;
    }

    *chr = s->data[s->top];
    s->top--;
    return 1;
}

int IsHuiwen(char* str) {
    // 判断回文数
    SeqStack stack;
    stack.top = -1;

    int len = strlen(str);
    int i;

    for (i = 0; i < len; i++) {
        Push(&stack, str[i]);
    }

    for (i = 0; i < len; i++) {
        char chr;
        Pop(&stack, &chr);
        if (chr != str[i]) {
            return 0;
        }
    }

    return 1;
}

int main() {
    char str[StackSize];
    printf("请输入字符串序列：");
    fgets(str, sizeof(str), stdin);

    // 去除末尾的换行符
    str[strcspn(str, "\n")] = '\0';

    if (IsHuiwen(str))
        printf("\n该字符串是回文字符串！\n");
    else
        printf("\n该字符串不是回文字符串！\n");

    return 0;
}
```

### 标准答案（考试用这个）

```C
int IsEmpty(SeqStack* s) {
    //判断栈是否为空
    return (s->top == -1 ? 1 : 0);
}

int IsHuiwen() {
    //判断回文数
    int i = 0, j = 0;
    char chr1, chr2, str[100];				  //定义数组，进栈时记录每个元素的次序
    SeqStack* s;							  //定义栈
    s = (SeqStack*)malloc(sizeof(SeqStack)); //给栈分配内存
    s->top = -1;							  //置栈顶指针为-1

    while ((chr1 = getchar()) != '\n') {      //输入字符串，按回车键结束
        Push_Stack(s, chr1); //入栈
        str[i] = chr1;		 //入栈的时候依次记录下每个字符对应的数组下标
        i++;
    }

    while (!IsEmpty(s)) {
        Pop_Stack(s, &chr2);  //出栈
        if (chr2 != str[j++]) //出栈的时候，将字符依次与入栈记录的字符比较
            return 0;		  //比较之后不相同，返回0
    }
    return 1;
}

int Push_Stack(SeqStack* s, char chr) {
    //进行入栈操作
    if (s->top == StackSize - 1)
        return 0; //栈满返回0
    s->top++;
    s->data[s->top] = chr; //入栈时，栈顶指针先+1，再输入数据
    return 1;
}

int Pop_Stack(SeqStack* s, char* chr) {
    //取出栈顶元素
    if (s->top == -1)
        return 0; //栈空返回0
    else {
        *chr = s->data[s->top];
        s->top--; //出栈时，先输出数据，栈顶指针再-1，
        return 1;
    }
}
```





## 队列的基本操作

1、假设以带头结点的循环链表表示队列，并且只设一个指针指向队尾元素结点（注意：不设头指针），试编写算法，实现队列的基本操作，包括初始化队列、判空操作、入队操作、出队操作。
2、使用链表实现队列数据结构。
3、程序应该能够处理任意数量的入队元素，并输出出队元素。

**示例：**

输入（第一个是元素个数）：

```
3
1
2
3
```

输出：

```
123
```

### 注意事项

1、队列的初始化需要分配一个头结点，并将队尾指针指向该头结点，形成一个空队列。
2、入队操作通过动态分配内存创建新的队列元素，并调整队尾指针。
3、出队操作通过删除队头元素，并将队尾指针指向新的队头元素。
4、程序中使用了动态内存分配，确保释放内存以防止内存泄漏。



#### 题目

```c
#include <stdio.h>
#include <stdlib.h>

typedef int ElemType;

typedef struct queue {
    ElemType data;
    struct queue *next;
} queue, *LinkQueue;

typedef struct {
    LinkQueue rear;
    int length;
} SqQueue;

void InitQueue(SqQueue *queue) {
    // 初始化队列，空队列
    // 请在此处编写代码
    
    
}

int EmptyQueue(SqQueue *queue) {
    // 判空操作
    // 请在此处编写代码
    
    
}

void EnQueue(SqQueue *queue, ElemType elem) {
    // 入队操作
    // 请在此处编写代码
    
    
}

void DelQueue(SqQueue *queue, ElemType *elem) {
    // 出队操作
    // 请在此处编写代码
    
    
}

int main() {
    int x, n;
    SqQueue Q;
    ElemType elem;
    InitQueue(&Q);

    // 判断队列是否为空
    if (EmptyQueue(&Q))
        printf("目前是一个空队列！\n");
    else
        printf("目前该队列中有元素，不为空！\n");

    // 入队
    printf("输入入队元素个数：");
    scanf("%d", &n);
    for (int i = 1; i <= n; i++) {
        printf("输入第%d个入队元素：", i);
        scanf("%d", &x);
        EnQueue(&Q, x);
    }

    printf("出队元素：");
    // 出队
    for (int j = 1; j <= n; j++) {
        DelQueue(&Q, &elem);
        printf("%d", elem);
    }
    return 0;
}
```

#### 答案

> 注意，它是个环！

> **注意：**如果把`EnQueue()`函数中的`malloc`操作中的`(sizeof(struct queue))`改为`(sizeof(queue))`则会编译报错。
>
> - **问题出现的地方**：
>
>     - 在你的 `EnQueue` 函数中，你尝试使用 `sizeof(queue)` 来获取 `queue` 数据类型所占内存的大小。
>     - 然而，`queue` 并不是一个具体的数据类型，它只是一个别名。
>     - 实际上，`queue` 在你的代码中是通过 `typedef` 定义的别名，代表的是 `struct queue` 这个结构体。
>
> - **解决方案**：
>
>     - 为了获取 `struct queue` 所占内存的大小，你应该使用 `sizeof(struct queue)` 或者 `sizeof(*queue->rear)`。
>
>     - 这样，`sizeof` 运算符就会返回正确的结构体 `queue` 所占内存的大小，而不会引发编译错误。
>
> 
>
> **不过这个错误只是在VS编译时会出现，并没有在Alpha中体现，标准答案也是直接**`sizeof(queue)`，我很不理解这个错误，平时不都是用别名吗。很抽象……
>
> 而且`InitQueue()`也不需要添加`struct`，但是`EmptyQueue()`不添加就会报错。

```c
void InitQueue(SqQueue* queue) {
    // 初始化队列，空队列
    queue->rear = (LinkQueue)malloc(sizeof(struct queue));
    queue->rear->next = queue->rear;
    queue->length = 0;
}

int EmptyQueue(SqQueue* queue) {
    // 判空操作
    return queue->length == 0;
}

void EnQueue(SqQueue* queue, ElemType elem) {
    // 入队操作
    LinkQueue newNode = (LinkQueue)malloc(sizeof(struct queue));
    newNode->data = elem;
    newNode->next = queue->rear->next;
    queue->rear->next = newNode;
    queue->rear = newNode;
    queue->length++;
}

void DelQueue(SqQueue* queue, ElemType* elem) {
    // 出队操作
    if (EmptyQueue(queue)) {
        printf("队列为空，无法出队！\n");
        return;
    }

    LinkQueue front = queue->rear->next->next;
    *elem = front->data;
    queue->rear->next->next = front->next;
    free(front);
    queue->length--;
}
```

### 标准答案

> 标准答案全程没有用过`length`，这个应该改进一下

```c
void InitQueue(SqQueue* queue) {
    // 初始化队列，空队列
    queue->rear = (LinkQueue)malloc(sizeof(queue));
    queue->rear->next = queue->rear;
}

int EmptyQueue(SqQueue* queue) {
    // 判空操作
    if (queue->rear->next == queue->rear)
        return 1;
    else
        return 0;
}

void EnQueue(SqQueue* queue, ElemType elem) {
    // 入队操作
    LinkQueue que = (LinkQueue)malloc(sizeof(queue));
    if (!que)
        return;
    
    que->data = elem;
    que->next = queue->rear->next;
    queue->rear->next = que;
    queue->rear = que;
}

void DelQueue(SqQueue* queue, ElemType* elem) {
    // 出队操作
    if (queue->rear->next == queue->rear)
        printf("为空队列！");
    
    LinkQueue front = queue->rear->next->next;
    *elem = front->data;
    queue->rear->next->next = front->next;
    free(front);
}
```





## 计算Ack的递归算法

已知`Ackermann`函数定义如下：

$$
\operatorname{Ack}(m, n)=\left\{\begin{array}{ll}n+1 & \text { 当 } m=0 \text { 时 } \\\operatorname{Ack}(m-1,1) & \text { 当 } m \neq 0, n=0 \text { 时 } \\\operatorname{Ack}\langle m-1, \operatorname{Ack}(m, n-1)\rangle & \text { 当 } m \neq 0, n \neq 0 \text { 时 }\end{array}\right.
$$

### 题目要求

1、编写一个程序，使用递归算法计算`Ackermann`函数。

2、用户输入两个非负整数 m 和 n。

3、计算并输出 Ackermann(m, n) 的结果。

**示例 1：**

输入

```
2,1
```

输出

```
Ack(2,1)=5
```

### 注意事项

1、`Ackermann`函数是一个非常特殊的递归函数，其增长速度极快，因此对于较大的输入值可能导致栈溢出或计算时间过长。
2、用户输入的 m 和 n 应为非负整数。



#### 题目

```c
#include<stdio.h>
 
int Ack(int m,int n){
    //Ack函数 ，递归算法 
    
}

int main(){
    int m,n,x;
    printf("请输入m,n：");
    scanf("%d,%d",&m,&n); 
    x=Ack(m,n);
    printf("Ack(%d,%d)=%d",m,n,x);
}
```

### 标准答案

```C
int Ack(int m, int n) {
    //Ack函数 ，递归算法 
    if (m == 0)
        return n + 1;
    else if (m != 0 && n == 0)
        return Ack(m - 1, 1);
    else if (m != 0 && n != 0)
        return Ack(m - 1, Ack(m, n - 1));
}
```





## 计算Ack的非递归算法

已知`Ackermann`函数定义如

$$
\operatorname{Ack}(m, n)=\left\{\begin{array}{ll}n+1 & \text { 当 } m=0 \text { 时 } \\\operatorname{Ack}(m-1,1) & \text { 当 } m \neq 0, n=0 \text { 时 } \\\operatorname{Ack}\langle m-1, \operatorname{Ack}(m, n-1)\rangle & \text { 当 } m \neq 0, n \neq 0 \text { 时 }\end{array}\right.
$$

### 题目要求

1、编写一个程序，使用非递归算法计算`Ackermann`函数。

2、用户输入两个非负整数 m 和 n。

3、计算并输出 Ackermann(m, n) 的结果。

**示例 1：**

输入

```
2,1
```

输出

```
Ack(2,1)=5
```

### 注意事项

1、`Ackermann`函数的非递归计算利用了一个二维数组，通过循环迭代计算得到结果。
2、用户输入的 m 和 n 应为非负整数。



#### 题目

```c
#include<stdio.h>
#define M 100
#define N 100
 
int Ack_re(int m, int n){
    //Ack函数，非递归 
    
}

int main(){
    int m,n,x;
    printf("请输入m,n：");
    scanf("%d,%d",&m,&n); 
    x=Ack_re(m,n);
    printf("Ack_re(%d,%d)=%d",m,n,x);
}
```

#### 答案
> 先看一个错误示范：这种会造成数组的越界访问，修改也就是将循环条件 `i <= m` 和 `j <= n` 修改为 `i < M` 和 `j < N`，确保在数组范围内进行循环。
>
> 但是这种感觉优化空间很大，毕竟它要循环M*N次，最后再取出一个值来，但是这个值很可能在数组的最前端，很可能绝大多数的循环都是无用的。

```c
int Ack_re(int m, int n) {
    int arr[M][N];
    int i, j;

    for (i = 0; i <= m; i++) {
        for (j = 0; j <= n; j++) {
            if (i == 0) {
                arr[i][j] = j + 1;
            }
            else if (j == 0) {
                arr[i][j] = arr[i - 1][1];
            }
            else {
                arr[i][j] = arr[i - 1][arr[i][j - 1]];
            }
        }
    }

    return arr[m][n];
}
```

> 正确答案：其实也就是用二维数组去存储每次Ack的执行结果，之后再需要用的时候就从二维数组中调用。（存储中间结果）

> 为了便于理解，画一个`9*9`的二维数组看一看，或者VS里面调试打断点监视。
>
> 可以看到很多地方是越界状态，所以为了满足有些计算要求，就只能扩大二维数组，但这也伴随着计算量的增加。当然，这儿的“越界”与错误示例的越界不同，错误示例的计算限制在了很小的空间中，就会导致越界是无法避免的。而在这里，我们可以通过扩大二维数组来避免越界。

```
    0  1  2  3  4  5  6  7  8
------------------------------
0|  1  2  3  4  5  6  7  8  9
1|  2  3  4  5  6  7  8  9 [ ]
2|  3  5  7  9  [   越  界   ]
3|  ..........................
4|  ..........................
5|  ..........................
6|  ..........................
7|  ..........................
8|  ..........................
```

**正确代码：**

```c
int Ack_re(int m, int n) {
    int arr[M][N];
    int i, j;

    for (i = 0; i < M; i++) {
        for (j = 0; j < N; j++) {
            if (i == 0) {
                arr[i][j] = j + 1;
            }
            else if (j == 0) {
                arr[i][j] = arr[i - 1][1];
            }
            else {
                arr[i][j] = arr[i - 1][arr[i][j - 1]];
            }
        }
    }

    return arr[m][n];
}

```

### 标准答案

```c
int Ack_re(int m, int n) {
    //Ack函数 ， 非递归
    int akm[M][N];
    int i, j;
    for (j = 0; j < N; j++)
        akm[0][j] = j + 1;
    for (i = 1; i < M; i++) {
        akm[i][0] = akm[i - 1][1];
        for (j = 1; j < N; j++)
            akm[i][j] = akm[i - 1][akm[i][j - 1]];
    }
    return (akm[m][n]);
}
```



## 判断序列B是否是序列A的子序列

**编程实现：** 两个整数序列`A=a1,a2,a3,...,am`和`B=b1,b2,b3,...,bn`已经存入两个单链表中，设计一个算法，判断字符串B是否是字符串A的子串。如果是，则输出`YES`；如果不是，则输出`NO`。

**示例：**

输入

```
3,2（两个链表的元素个数）
1（A链表的第 1 个元素）
2（A链表的第 2 个元素）
3（A链表的第 3 个元素）
1（B链表的第 1 个元素）
2（B链表的第 2 个元素）
```

输出

```
YES
```



#### 题目

```C
#include <stdio.h>
#include <stdlib.h>

typedef struct LNode {
    int data;
    struct LNode *next;
} LNode, *LinkList;

void Create_LinkList(LinkList *L, int n) {
    // 创建链表
    // 请在此处编写代码
    
    
}

void print_LinkList(LinkList L) {
    // 输出链表
    // 请在此处编写代码
    
    
}

int Sub_Sequence(LinkList La, LinkList Lb) {
    // 判断字符串B是否是字符串A的子串
    // 请在此处编写代码
    
    
}

int main() {
    // 创建2个链表
    LinkList La, Lb;
    // 输入数据
    int m, n;
    printf("两个链表的元素个数分别为：");
    scanf("%d,%d", &m, &n); // 输入以逗号分隔开
    printf("请输入第1个链表的数据：\n");
    Create_LinkList(&La, m);
    printf("请输入第2个链表的数据：\n");
    Create_LinkList(&Lb, n);
    if (Sub_Sequence(La, Lb))
        printf("\nYES\n");
    else
        printf("\nNO\n");
    return 0;
}
```

#### 答案

> 在`Sub_Sequence`函数中，使用两个指针`pa`和`pb`分别指向链表A和链表B的头节点。
> 通过比较`pa`和`pb`指向的节点的值，若相等，则两个指针同时后移一位；若不相等，则将`pb`重新指向链表B的头节点，并将pa后移一位。
> 重复上述步骤，直到链表A遍历完毕或者链表B遍历完毕。
> 若链表B遍历完毕，则说明B是A的子串；否则，B不是A的子串。
>
> 
>
> `print_LinkList()`也没有调用呀，怎么还让补全呢哈哈哈哈

```c
void Create_LinkList(LinkList* L, int n) {
    // 创建链表
    *L = (LinkList)malloc(sizeof(LNode));
    (*L)->next = NULL;
    LinkList p = *L;
    int i, temp;
    for (i = 0; i < n; i++) {
        scanf("%d", &temp);
        LinkList newNode = (LinkList)malloc(sizeof(LNode));
        newNode->data = temp;
        newNode->next = NULL;
        p->next = newNode;
        p = p->next;
    }
}

void print_LinkList(LinkList L) {
    // 输出链表
    LinkList p = L->next;
    while (p) {
        printf("%d ", p->data);
        p = p->next;
    }
    printf("\n");
}

int Sub_Sequence(LinkList La, LinkList Lb) {
    // 判断字符串B是否是字符串A的子串
    LinkList pa = La->next;
    LinkList pb = Lb->next;
    while (pa && pb) {
        if (pa->data == pb->data) {
            pa = pa->next;
            pb = pb->next;
        }
        else {
            pa = pa->next;
            pb = Lb->next;
        }
    }

    if (pb == NULL) {
        return 1; // B是A的子串
    }
    else {
        return 0; // B不是A的子串
    }
}
```

> 其实`Sub_Sequence()`貌似有点逻辑上的错误，就是不匹配的时候`pa`就直接递归到`pa->next`了，而正确做法应该是递归到`pa`开始匹配的下一个字符（不是匹配结束后的下一个字符）
>
> 但是这样子居然通过检查了。
>
> 但是我们还是修改一下吧，再引入一个`LinkList temp = pa;`

```c
int Sub_Sequence(LinkList La, LinkList Lb) {
    // 判断字符串B是否是字符串A的子串
    LinkList pa = La->next;
    LinkList pb = Lb->next;

    LinkList temp = pa;
    while (temp && pb) {
        if (temp->data == pb->data) {
            temp = temp->next;
            pb = pb->next;
        }
        else {
            temp = pa->next;
            pa = pa->next;
            pb = Lb->next;
        }
    }

    if (pb == NULL) {
        return 1; // B是A的子串
    }
    else {
        return 0; // B不是A的子串
    }
}
```

### 标准答案

```c
void Create_LinkList(LinkList* L, int n) {
    // 创建链表
    *L = (LinkList)malloc(sizeof(LNode)); // 创建头结点
    (*L)->next = NULL;
    LinkList rear; // 尾指针
    rear = *L;     // 指向头结点
    for (int i = 0; i < n; i++) {
        LinkList p = (LinkList)malloc(sizeof(LNode)); // 给新结点创建空间
        p->next = NULL;
        printf("单链表中第%2d个元素是：", i + 1);   // 下标是从0开始的，得加1
        scanf("%d", &p->data);
        rear->next = p; // 新结点插入链表尾部
        rear = p;       // rear指向新的尾节点
    }
}

void print_LinkList(LinkList L) {
    // 输出链表
    LinkList p = L;
    while (p->next != NULL) {
        p = p->next; // 跳过头结点输出下一个结点中存储的数值
        printf("%d", p->data);
    }
}

int Sub_Sequence(LinkList La, LinkList Lb) {
    // 判断字符串B是否是字符串A的子串

    LinkList pa = La, pb = Lb, first = pa; // first记录每一次的子串首元素
    while (pa && pb) {
        if (pa->data == pb->data) {
            pa = pa->next;
            pb = pb->next;
        }
        else {
            first = first->next; // 下一个first验证
            pa = first;          // pa从first遍历
            pb = Lb;             // pb从开始遍历
        }
    }
    if (pb == NULL) // A未结束，B结束了
        return 1;    // YES
    else             // A完了，B没完
        return 0;    // NO
}
```

