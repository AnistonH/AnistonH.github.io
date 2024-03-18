# Alpha 栈和队列

> 编写于：2024-03-18  （未完待续）

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

```c

```

