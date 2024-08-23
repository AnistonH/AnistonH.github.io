# Alpha 单链表

## 打印顺序表

请编写一个程序，完善函数`PrintList`，实现创建一个顺序表，并实现打印顺序表的功能。

**示例输出**

```
1
2
3
```



#### 题目

```c
#include <stdio.h>
#include <stdlib.h>

#define MAXSIZE 100
typedef int ElemType;

typedef struct{
    ElemType data[MAXSIZE];
    int length;
}SqList;

void PrintList(SqList L){
    //请在此处编写代码
    
    
    
}

int main(){
    SqList L;
    L.data[0] = 1;
    L.data[1] = 2;
    L.data[2] = 3;
    L.length = 3;
    PrintList(L);
    return 0;
}
```

### 标准答案

```c
void PrintList(SqList L){
    for(int i = 0; i < L.length; i++){
        printf("%d\n",L.data[i]);
    }
}
```





## 顺序表的插入

请编写一个程序，完善函数`ListInsert`和`PrintList`，实现在顺序表中的指定位置插入元素的功能，并打印输出顺序表。

**示例输出**

```
Insert success
1
4
2
3
```



#### 题目

```C
#include <stdio.h>
#include <stdbool.h>

#define MAXSIZE 100
typedef int ElemType;

typedef struct {
    ElemType data[MAXSIZE];
    int length;
} SqList;

bool ListInsert(SqList *L, int i, ElemType e) {
    //请在此处编写代码
    
    
}

void PrintList(SqList L) {
    //请在此处编写代码
    
    
}

int main() {
    SqList L;
    L.data[0] = 1;
    L.data[1] = 2;
    L.data[2] = 3;
    L.length = 3;
    bool ret = ListInsert(&L, 2, 4);
    if (ret) {
        printf("Insert success\n");
        PrintList(L);
    } else {
        printf("Insert is false\n");
    }
    return 0;
}
```

### 标准答案

```c
bool ListInsert(SqList* L, int i, ElemType e) {
    if (i > L->length + 1 || i < 1) {
        return false;
    }
    if (L->length >= MAXSIZE) {
        return false;
    }
    for (int j = L->length; j >= i; j--) {
        L->data[j] = L->data[j - 1];
    }
    L->data[i - 1] = e;
    L->length++;
    return true;
}

void PrintList(SqList L) {
    for (int i = 0; i < L.length; i++) {
        printf("%d\n", L.data[i]);
    }
}
```





## 删除顺序表元素

请编写一个程序，完善函数`ListDelete`和`PrintList`，实现在顺序表的指定位置删除元素的功能，并打印输出顺序表。

**示例输出**

```
Delete success
1
3
```



#### 题目

```c
#include <stdio.h>
#include <stdbool.h>

#define MAXSIZE 100
typedef int ElemType;

typedef struct {
    ElemType data[MAXSIZE];
    int length;
} SqList;

bool ListDelete(SqList *L, int i, ElemType *e) {
    //请在此处编写代码
    
    
}

void PrintList(SqList L) {
    //请在此处编写代码
    
    
}

int main() {
    SqList L;
    L.data[0] = 1;
    L.data[1] = 2;
    L.data[2] = 3;
    L.length = 3;
    ElemType e;
    bool ret = ListDelete(&L, 2, &e);
    if (ret) {
        printf("Delete success\n");
        PrintList(L);
    } else {
        printf("Delete is false\n");
    }
    return 0;
}
```

### 标准答案

```c
bool ListDelete(SqList* L, int i, ElemType* e) {
    if (i < 1 || i > L->length) {
        return false;
    }
    if (L->length == 0) {
        return false;
    }
    
    *e = L->data[i - 1];
    for (int j = i; j < L->length; j++) {
        L->data[j - 1] = L->data[j];
    }
    
    L->length--;
    
    return true;
}

void PrintList(SqList L) {
    for (int i = 0; i < L.length; i++) {
        printf("%d\n", L.data[i]);
    }
}
```





## 按值查找顺序表元素

请编写一个程序，完善函数`LocateElem`，实现在顺序表中查找指定元素位置的功能，并打印输出顺序表。

**示例输出**

```
Locate success
Position: 2
```



#### 题目

```c
#include <stdio.h>
#include <stdbool.h>

#define MAXSIZE 100
typedef int ElemType;

typedef struct{
    ElemType data[MAXSIZE];
    int length;
}SqList;

int LocateElem(SqList L, ElemType e){
    //请在此处编写代码
    
    
    
}

int main(){
    SqList L;
    L.data[0] = 1;
    L.data[1] = 2;
    L.data[2] = 3;
    L.length = 3;
    int ret = LocateElem(L, 2);
    if(ret != -1){
        printf("Locate success\n");
        printf("Position: %d", ret);
    } else{
        printf("Locate is false\n");
    }
    return 0;
}
```

### 标准答案

```c
int LocateElem(SqList L, ElemType e) {
    for (int i = 0; i < L.length; i++) {
        if (L.data[i] == e) {
            return i + 1;
        }
    }
    return -1;
}
```





## 单链表的合并

> 涉及：单链表的合并  删除重复数据  转置链表
>
> 这个题目简单点做就是头插。复杂点做就先从小到大合并，然后删除重复数据，然后逆转元素了（去重也可以包含在合并中）。
>
> 另外说明一下，非注释的代码将合并和删除操作合二为一了，但是也一定要看看删除操作，后面的题目“合并递增有序链表”也是同样的操作

**要求：**两个单链表’A’和’B’，均是按元素值"递增有序"排列，请编写算法将’A’表和’B’表按元素值"递减有序"，归并到’C’表。'C’表中针对’A’表和’B’表重复的元素，只保留一个。打印输出’C’表中的元素。（补全`merge()`）

**示例**

```
A表：{1,3,5,6,8,9}
B表：{2,3,4,5,6,7}
```

**输出**

```
9 8 7 6 5 4 3 2 1
```



#### 题目：

```c
#include <stdio.h>
#include <stdlib.h>

typedef int ElemType;
//单链表
typedef struct LNode{
    ElemType data;
    struct LNode *next;
}LNode, *LinkList;

//请完成该函数，实现A和B，按递减合并为C，并且A和B中相同的元素，只出现一次 
int merge(LinkList A, LinkList B, LinkList C)
{  
   
    return 1;
}


int main()
{

    ElemType ad[6] = {1,3,5,6,8,9};
    ElemType bd[6] = {2,3,4,5,6,7};
	LinkList A,pA,B,pB,C,pC,l;
	int i; 
    
    //创建单链表A并赋值 
    A = (LinkList)malloc(sizeof(LNode));//头结点 
    A->next = NULL;
	pA = A;
    for(i = 0; i < 6; i++){
        l = (LinkList)malloc(sizeof(LNode));
        l->data = ad[i];
        l->next = NULL;
        pA->next = l;
        pA = pA->next;
    }    
    
    //创建单链表B并赋值
    B = (LinkList)malloc(sizeof(LNode));//头结点 
    B->next = NULL;
	pB = B;
    for(i = 0; i < 6; i++){
        l = (LinkList)malloc(sizeof(LNode));
        l->data = bd[i];
        l->next = NULL;
        pB->next = l;
        pB = pB->next;
    }    
 
	C = (LinkList)malloc(sizeof(LNode));//头结点 
    C->next = NULL;
        //merge函数，单链表合并功能
    merge(A,B,C);
    
    //打印C的元素 
   	pC = C->next;
	for(; pC!=NULL; pC = pC->next){
         printf("%d ",pC->data);
    }

    return 0;
}
```

#### 答案：

##### 方法一：直接头插

```c
int merge(LinkList A, LinkList B, LinkList C)
{
    LinkList pA = A->next;
    LinkList pB = B->next;
    LinkList pC = C;
    pC->next = NULL;

    LinkList temp = (LinkList)malloc(sizeof(LNode));
    temp->next = NULL;

    while (pA && pB) {
        if (pA->data < pB->data) {
            temp = pA->next;
            pA->next = pC -> next;
            pC->next = pA;
            pA = temp;
        }
        else if (pA->data > pB->data) {
            temp = pB->next;
            pB->next = pC->next;
            pC->next = pB;
            pB = temp;
        }
        else {
            temp = pA->next;
            pA->next = pC->next;
            pC->next = pA;
            pA = temp;
            pB = pB->next;
        }
    }

    while (pA) {
        temp = pA->next;
        pA->next = pC->next;
        pC->next = pA;
        pA = temp;
    }

    while (pB) {
        temp = pB->next;
        pB->next = pC->next;
        pC->next = pB;
        pB = temp;
    }

    return 1;
}
```



##### 方法二：尾插+逆转

```c
#include <stdio.h>
#include <stdlib.h>

typedef int ElemType;
//单链表
typedef struct LNode {
    ElemType data;
    struct LNode* next;
} LNode, * LinkList;

//请完成该函数，实现A和B，按递减合并为C，并且A和B中相同的元素，只出现一次 
int merge(LinkList A, LinkList B, LinkList C) {

    // 1.按升序串起来
    LinkList pA = A->next;
    LinkList pB = B->next;
    LinkList pC = C;

    while (pA && pB) {
        if (pA->data < pB->data) {		
            pC->next = pA;
            pA = pA->next;
        }
        else if (pA->data > pB->data) {
            pC->next = pB;
            pB = pB->next;
        }
        else { // 跳过重复元素
            pC->next = pA;
            pA = pA->next;
            pB = pB->next;
            //continue;
        }
        pC = pC->next;
        pC->next = NULL;
    }

    pC->next = pA ? pA : pB;


    //// 1.按升序合并起来
    //while (pA && pB) {
    //    if (pA->data < pB->data) {
    //        pC->next = pA;
    //        pA = pA->next;
    //    }
    //    else {
    //        pC->next = pB;
    //        pB = pB->next;
    //    }
    //    pC = pC->next;
    //}
    //pC->next = pA ? pA : pB;

    //// 2.删除重复的值
    //pC = C;

    //while (pC && pC->next) {
    //    LinkList next = pC->next;
    //    if (pC->data == next->data) {
    //        pC->next = next->next;
    //        free(next);
    //    }
    //    else {
    //        pC = pC->next;
    //    }
    //}


    // 3. 链表逆转
    LinkList prev = NULL;
    LinkList curr = C->next;
    LinkList next;
    while (curr) {
        next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }
    C->next = prev;

    return 1;
}


int main() {
    ElemType ad[6] = { 1, 3, 5, 6, 8, 9 };
    ElemType bd[6] = { 2, 3, 4, 5, 6, 7 };
    LinkList A, pA, B, pB, C, pC, l;
    int i;

    //创建单链表A并赋值 
    A = (LinkList)malloc(sizeof(LNode));  //头结点 
    A->next = NULL;
    pA = A;
    for (i = 0; i < 6; i++) {
        l = (LinkList)malloc(sizeof(LNode));
        l->data = ad[i];
        l->next = NULL;
        pA->next = l;
        pA = pA->next;
    }

    //创建单链表B并赋值
    B = (LinkList)malloc(sizeof(LNode));  //头结点 
    B->next = NULL;
    pB = B;
    for (i = 0; i < 6; i++) {
        l = (LinkList)malloc(sizeof(LNode));
        l->data = bd[i];
        l->next = NULL;
        pB->next = l;
        pB = pB->next;
    }

    C = (LinkList)malloc(sizeof(LNode));  //头结点 
    C->next = NULL;

    //merge函数，单链表合并功能
    merge(A, B, C);

    //打印C的元素 
    pC = C->next;
    for (; pC != NULL; pC = pC->next) {
        printf("%d ", pC->data);
    }

    return 0;
}
```

### 标准答案（不太好）

```c
int merge(LinkList A, LinkList B, LinkList C)
{
    LinkList pa, pb, qa, qb, pc, qc;
    pa = A;
    pb = B;
    qa = pa; // 保存 pa 的前驱指针
    qb = pb; // 保存 pb 的前驱指针
    pa = pa->next;
    pb = pb->next;
    qc = C;
    while (pa && pb) {
        if (pa->data < pb->data) {
            pc = (LinkList)malloc(sizeof(LNode));
            pc->data = pa->data;
            pc->next = qc->next;
            qc->next = pc;
            qa = pa;
            pa = pa->next;
        }
        else {
            pc = (LinkList)malloc(sizeof(LNode));
            pc->data = pb->data;
            pc->next = qc->next;
            qc->next = pc;
            if (pa->data == pb->data) {
                qa = pa;
                pa = pa->next;
            }
            qb = pb;
            pb = pb->next;
        }
    }
    while (pa) {
        pc = (LinkList)malloc(sizeof(LNode));
        pc->data = pa->data;
        pc->next = qc->next;
        qc->next = pc;
        qa = pa;
        pa = pa->next;
    }
    while (pb) {
        pc = (LinkList)malloc(sizeof(LNode));
        pc->data = pb->data;
        pc->next = qc->next;
        qc->next = pc;
        qb = pb;
        pb = pb->next;
    }
    return 1;
}
```





## 单链表的逆置

编写程序，对单链表进行逆置。

**示例**

```
逆置前：
9 8 7 6 5 4 3 2 1 0 
逆置后：
0 1 2 3 4 5 6 7 8 9
```



#### 题目：

```C
#include <stdio.h>
#include <stdlib.h>

typedef struct LNode{
    int data;
    struct LNode *next;
}LNode, *LinkList;


void reverse(LinkList L)
{
    //请在此处编写单链表的逆置代码

    
}

int main()
{
    LinkList L;
    LinkList p;
    int n = 10;
    L = (LinkList)malloc(sizeof(LNode));
    L->next = NULL;
    for(int i = 0; i < n; i++){
        p = (LinkList)malloc(sizeof(LNode));
        p->data = i;
        p->next = L->next;
        L->next = p;
    }
    printf("逆置前：\n");
    for(p = L->next; p!=NULL; p = p->next){
        printf("%d ",p->data);
    }
    reverse(L);
    printf("\n逆置后：\n");
    for(p = L->next; p!=NULL; p = p->next){
        printf("%d ",p->data);
    }
    return 0;
}
```

#### 答案：

> 就是上一题中的逆转

```c
void reverse(LinkList L)
{
    LinkList prev = NULL;
    LinkList curr = L->next;
    LinkList next = NULL;

    while (curr) {
        next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }
    L->next = prev;
}
```

### 标准答案（不太好）

```c
void reverse(LinkList L)
{
    LinkList p, q;
    p = L;
    p = p->next;
    L->next = NULL;
    while (p) {
        q = p;
        p = p->next;
        q->next = L->next;
        L->next = q;
    }
}
```





## 循环链表的应用

100个小朋友围成一个圆圈，按顺序从编号1到编号100，做游戏，从第一个小朋友开始从1顺序循环报数，报到3的倍数小朋友自动淘汰，然后下一个小朋友继续报数，直到只剩下最后1个小朋友，请问最后剩下的小朋友是几号小朋友。

**编写程序：** 利用循环链表，输出最后剩余小朋友的编号。

**示例输出：**91



#### 题目：

```C
#include <stdio.h>
#include <stdlib.h>

typedef int ElemType;
//单循环链表
typedef struct LNode{
    ElemType data;
    struct LNode *next;
}LNode, *LinkList;

//完成此函数，返回剩余小朋友编号 
int game(LinkList A)
{
	
}


int main()
{
	LinkList A,pA,pPre,l;
	int n=100;//小朋友个数 
	int i; 
    
    //创建单循环链表 
    A = (LinkList)malloc(sizeof(LNode));//头结点 
    A->next = A;
	pA = A;
    for(i = 1; i <= n; i++){
        l = (LinkList)malloc(sizeof(LNode));
        l->data = i;
        l->next = pA->next;
        pA->next = l;
        pA = pA->next;
    }    
    
    int result = game(A);
    
    //打印剩余的元素 
    printf("%d ",result);

    return 0;
}
```

#### 错误示范：

```c
int game(LinkList A)
{
    LNode *p = A->next; // 从第一个小朋友开始
    LNode *pre = A; // 记录前一个小朋友，便于删除操作
    int count = 1; // 记录报数
    while (p != p->next) { // 当只剩下一个小朋友时停止
        if (count % 3 == 0) { // 当报数为3的倍数时，删除当前节点
            pre->next = p->next;
            free(p);
            p = pre->next;
        } else {
            pre = p;
            p = p->next;
        }
        count++;
    }
    return p->data; // 返回最后一个小朋友的编号
}

```

> 这段代码的错误在于：循环链表的时候哨兵位并没有被删除，就导致哨兵也参与到了一圈一圈的循环之中，所以只要再循环开始之前将哨兵去掉就好了
>
> 一定要看明白题啊！！

#### 答案：

> 看**标准答案**吧，这个写的确实挺烂……

```c
 int game(LinkList A)
{
    // 查询小孩数量
    LinkList curr = A->next->next; // A是哨兵
    int num = 0;
    while (curr->data != 1) {
        curr = curr->next;
        num++;
    }
    // num为小孩数量


    // 删除哨兵位
    // 这里是通过小孩数量进行循环
    curr = A->next; // A是哨兵，注意这儿curr进行了重定义
    while (--num) { // 需要进行 num-1 次循环
        curr = curr->next;
    } 
    // 现在curr中的data值是100，下一个就是哨兵位
    LinkList temp = curr->next;
    curr->next = temp->next;
    curr = curr->next; // 现在curr中data的值是1
    free(temp);


    // 开始删除操作
    LinkList p = curr; // 从第一个小朋友开始
    LinkList pre = A; // 记录前一个小朋友，便于删除操作
    int count = 1; // 记录报数
    while (p != p->next) { // 当只剩下一个小朋友时停止
        if (count % 3 == 0) { // 当报数为3的倍数时，删除当前节点
            pre->next = p->next;
            free(p);
            p = pre->next;
        }
        else {
            pre = p;
            p = p->next;
        }
        count++;
    }

    return p->data; // 返回最后一个小朋友的编号
}
```

### 标准答案（考试用这个）

> 我们的代码改进空间在于对哨兵位的处理，我们的代码有一大堆操作去删除哨兵位，而在标准答案中，只使用了一个`if`去判断当前是否哨兵位，是的话往前递进一下跳过。
>
> 注意：`A`是一直没有变化的，一直都是哨兵位。

```c
int game(LinkList A)
{
    // 声明两个指针变量pA和pPre，分别指向链表A中的下一个节点和当前节点
    LinkList pA, pPre;
    pA = A->next; // 将pA指向链表A的下一个节点
    pPre = A; // 将pPre指向链表A的当前节点
    
    int bs = 1; // 初始化bs为1，用于记录当前处理的节点位置
    while (A->next->next != A) { // 当剩余节点数不只一个时执行循环
        if (bs % 3 == 0) { // 判断是否为第3个节点
            // 删除一个结点
            pPre->next = pA->next; // 将pA的下一个节点赋给pPre的下一个节点
            free(pA); // 释放pA指向的节点内存
            pA = pPre->next; // 将pA指向下一个节点
            
            if (pA == A) { // 如果pA为哨兵位
                pA = A->next; // 将pA指向链表A的第一个节点
                pPre = A; // 将pPre指向链表A的哨兵位
            }
        }
        else {
            pPre = pA; // 将pPre指向pA
            pA = pA->next; // 将pA指向下一个节点
            
            if (pA == A) { // 如果pA为哨兵位
                pA = A->next; // 将pA指向链表A的第一个节点
                pPre = A; // 将pPre指向链表A的哨兵位
            }
        }
        bs = bs + 1; // bs自增1
    }
    
    return A->next->data; // 返回链表A中哨兵位下一个节点的数据
}
```





## 合并递增有序链表

**将两个递增的有序链表合并为一个递增的有序链表**

**编程实现：** 将两个递增的有序链表合并为一个递增的有序链表。要求结果链表仍使用原来两个链表的存储空间，不另外占用其他的存储空间。表中`不允许有重复`的数据。

**函数定义**

```
/* 创建链表：此函数的作用是创建链表 L，链表的长度为 n */
void Create_LinkList(LinkList *L, int n)

/* 合并链表：此函数的作用是将两个链表 La 和 Lb 合并为链表 Lc */
void MergeList(LinkList La, LinkList Lb, LinkList *Lc)

/* 输出数据：此函数的作用是将链表 L 的数据输出 */
void print_LinkList(LinkList L)
```

**输入**

```
3,3（两个链表的元素个数）
1（第 1 个链表的第 1 个元素）
2（第 1 个链表的第 2 个元素）
3（第 1 个链表的第 3 个元素）
3（第 2 个链表的第 1 个元素）
4（第 2 个链表的第 2 个元素）
5（第 2 个链表的第 3 个元素）
```

**输出**

```
合并后的链表为: 12345
```



#### 题目：

> 合并函数中的两种操作，和第一题“单链表的合并”中一模一样。

```c
#include <stdio.h>
#include <stdlib.h>

// 定义
typedef struct {
    int date;
} Date;

typedef struct LNode {
    Date elem;
    struct LNode *next;
} LNode, *LinkList;

void Create_LinkList(LinkList *L, int n) {
    // 创建链表
    //请在此处编写代码
    
    
    
}

void MergeList(LinkList La, LinkList Lb, LinkList *Lc) {
    // 合并链表La和Lb，合并后的新表使用头指针Lc指向
    //请在此处编写代码
    
    
    
}

// 输出数据
void print_LinkList(LinkList L) {
    //请在此处编写代码
    
    
    
}

int main() {
    // 创建2个链表
    LinkList La, Lb, Lc;

    // 输入数据
    int sum01, sum02;
    printf("两个链表的元素个数分别为: ");
    scanf("%d,%d", &sum01, &sum02); // 输入以英文逗号分隔开
    printf("请输入第 1 个链表的数据:\n");
    Create_LinkList(&La, sum01);
    printf("请输入第 2 个链表的数据:\n");
    Create_LinkList(&Lb, sum02);

    // 合并链表并输出
    MergeList(La, Lb, &Lc);
    printf("\n合并后的链表为: ");
    print_LinkList(Lc);

    return 0;
}
```

#### 答案：

```c
void Create_LinkList(LinkList* L, int n) {
    *L = (LinkList)malloc(sizeof(LNode)); // 为头节点分配内存
    (*L)->next = NULL; // 初始化头节点的下一个指针为NULL

    LinkList p = *L; // 用于遍历链表的指针

    //printf("请输入链表的数据:\n");
    for (int i = 0; i < n; i++) {
        p->next = (LinkList)malloc(sizeof(LNode)); // 为下一个节点分配内存
        p = p->next; // 移动到新创建的节点
        //printf("请输入第 %d 个元素的值: ", i + 1);
        scanf("%d", &p->elem.date); // 读取当前节点的数据
    }

    p->next = NULL; // 将最后一个节点的下一个指针设为NULL
}


void MergeList(LinkList La, LinkList Lb, LinkList* Lc) {
    LinkList pa = La->next;  // 从La的第一个节点开始
    LinkList pb = Lb->next;  // 从Lb的第一个节点开始

    *Lc = La;  // 将Lc指向La的头节点
    LinkList pc = *Lc;  // 初始化一个指向Lc头节点的指针

    while (pa && pb) {
        if (pa->elem.date < pb->elem.date) {
            pc->next = pa;  // 将来自La的节点链接到Lc
            pa = pa->next;  // 移动到La的下一个节点
        }
        else if (pa->elem.date > pb->elem.date) {
            pc->next = pb;  // 将来自Lb的节点链接到Lc
            pb = pb->next;  // 移动到Lb的下一个节点
        }
        else {
            pc->next = pa;
            pa = pa->next;  // 移动到La的下一个节点
            pb = pb->next;  // 移动到Lb的下一个节点
        }
        pc = pc->next;  // 移动到Lc的下一个节点
        pc->next = NULL;
    }

    // 如果La或Lb中还有剩余的节点，将它们链接到Lc中
    pc->next = pa ? pa : pb;
}

// 输出数据
void print_LinkList(LinkList L) {
    LinkList p = L->next; // 从第一个节点开始
    while (p) {
        printf("%d ", p->elem.date); // 打印数据
        p = p->next; // 移动到下一个节点
    }
    printf("\n");
}
```

### 标准答案（不太好）

```c
void Create_LinkList(LinkList* L, int n) {
    // 创建链表
    *L = (LinkList)malloc(sizeof(LNode)); // 创建头结点
    (*L)->next = NULL;
    LinkList rear; // 尾指针
    rear = *L;      // 指向头结点
    for (int i = 0; i < n; i++) {
        LinkList p = (LinkList)malloc(sizeof(LNode)); // 给新结点创建空间
        p->next = NULL;
        printf("单链表中第 %d 个元素是: ", i + 1); // 下标是从0开始的，得加1
        scanf("%2d", &p->elem.date);
        rear->next = p; // 新结点插入链表尾部
        rear = p;       // rear指向新的尾节点
    }
}

void MergeList(LinkList La, LinkList Lb, LinkList* Lc) {
    // 合并链表La和Lb，合并后的新表使用头指针Lc指向
    LinkList pa = La->next;
    LinkList pb = Lb->next;
    // pa和pb分别是链表La和Lb的工作指针,初始化为相应链表的第一个结点
    LinkList pc;
    *Lc = pc = La; // 用La的头结点作为Lc的头结点
    while (pa && pb) {
        if (pa->elem.date < pb->elem.date) {
            // 取较小者La中的元素，将pa链接在pc的后面，pa指针后移
            pc->next = pa;
            pc = pa;
            pa = pa->next;
        }
        else if (pa->elem.date > pb->elem.date) {
            // 取较小者Lb中的元素，将pb链接在pc的后面，pb指针后移
            pc->next = pb;
            pc = pb;
            pb = pb->next;
        }
        else {
            // 相等时取La中的元素，删除Lb中的元素
            pc->next = pa;
            pc = pa;
            pa = pa->next;
            LinkList q = pb->next;
            free(pb);
            pb = q;
        }
    }
    pc->next = pa ? pa : pb; // 插入剩余段
    free(Lb);               // 释放Lb的头结点
}

// 输出数据
void print_LinkList(LinkList L) {
    LinkList p = L;
    p = L->next;
    while (p) {
        printf("%d", p->elem.date);
        p = p->next;
    }
}
```





## 两个链表的交集

**编程实现：** 已知两个链表 A 和 B 分别表示两个集合，其元素递增排列。请设计一个算法，用于求出 A 与 B 的交集，并存放于 A 链表中。**(这题说错了，应该是存放于 C 链表中)**

**函数定义**

```
/* 创建链表：此函数的作用是创建链表 L，链表的长度为 n */
void Create_LinkList(LinkList *L, int n)

/* 求出交集：此函数的作用是求出两个链表 La 和 Lb 的交集 Lc */
void Mix(LinkList *La, LinkList *Lb, LinkList *Lc)

/* 输出数据：此函数的作用是将链表 L 的数据输出 */
void print_LinkList(LinkList L)
```

**输入**

```
3,3（两个链表的元素个数）
1（第 1 个链表的第 1 个元素）
2（第 1 个链表的第 2 个元素）
3（第 1 个链表的第 3 个元素）
3（第 2 个链表的第 1 个元素）
4（第 2 个链表的第 2 个元素）
5（第 2 个链表的第 3 个元素）
```

**输出**

```
合并后的链表为: 3
```



####  题目：

> 这道题里面可能指针的操作有一点点……
>
> 比如`    *L = (LinkList)malloc(sizeof(LNode));` 和`(*L)->next = NULL;`
>
> 双重指针，此时的L是`Node**`，所以就先解开一层，对`Node*`操作（`*L`类型是`Node*`）

```####c
#include <stdio.h>
#include <stdlib.h>

typedef struct LNode {
    int data;
    struct LNode *next;
} LNode, *LinkList;

void Create_LinkList(LinkList *L, int n) {
    //请在此处编写代码
    
    
    
}

void print_LinkList(LinkList L) {
    //请在此处编写代码
    
    
    
}

void Mix(LinkList *La, LinkList *Lb, LinkList *Lc) {
    //请在此处编写代码
    
    
    
}

int main() {
    // 创建2个链表
    LinkList La, Lb, Lc;
    // 输入数据
    int sum01, sum02;
    printf("两个链表的元素个数分别为: ");
    scanf("%d,%d", &sum01, &sum02); // 输入以英文逗号分隔开
    printf("请输入第 1 个链表的数据:\n");
    Create_LinkList(&La, sum01);
    printf("请输入第 2 个链表的数据:\n");
    Create_LinkList(&Lb, sum02);

    Mix(&La, &Lb, &Lc);
    printf("\n合并后的链表为: ");
    print_LinkList(Lc);
    return 0;
}
```

#### 答案：

```c
void Create_LinkList(LinkList* L, int n) {
    *L = (LinkList)malloc(sizeof(LNode));
    (*L)->next = NULL;
    LinkList p = *L;

    //printf("请输入链表的数据:\n");
    for (int i = 0; i < n; i++) {
        LinkList newNode = (LinkList)malloc(sizeof(LNode));
        scanf("%d", &(newNode->data));
        newNode->next = NULL;
        p->next = newNode;
        p = p->next;
    }
}

void print_LinkList(LinkList L) {
    LinkList p = L->next;
    while (p != NULL) {
        printf("%d ", p->data);
        p = p->next;
    }
    printf("\n");
}

void Mix(LinkList* La, LinkList* Lb, LinkList* Lc) {
    LinkList pa = (*La)->next;
    LinkList pb = (*Lb)->next;
    LinkList pc = (LinkList)malloc(sizeof(LNode));
    pc->next = NULL;
    *Lc = pc;

    while (pa != NULL && pb != NULL) {
        if (pa->data < pb->data) {
            pa = pa->next;
        }
        else if (pa->data > pb->data) {
            pb = pb->next;
        }
        else {
            LinkList newNode = (LinkList)malloc(sizeof(LNode));
            newNode->data = pa->data;
            newNode->next = NULL;
            pc->next = newNode;
            pc = pc->next;
            pa = pa->next;
            pb = pb->next;
        }
    }
}
```

### 标准答案（不太好）

```c
void Create_LinkList(LinkList* L, int n) {
    *L = (LinkList)malloc(sizeof(LNode)); // 创建头结点
    (*L)->next = NULL;
    LinkList rear; // 尾指针
    rear = *L;     // 指向头结点
    for (int i = 0; i < n; i++) {
        LinkList p = (LinkList)malloc(sizeof(LNode)); // 给新结点创建空间
        p->next = NULL;
        printf("单链表中第 %d 个元素是: ", i + 1); // 下标是从0开始的，得加1
        scanf("%d", &p->data);
        rear->next = p; // 新结点插入链表尾部
        rear = p;       // rear指向新的尾节点
    }
}

void print_LinkList(LinkList L) {
    LinkList p = L;
    while (p->next != NULL) {
        p = p->next; // 跳过头结点输出下一个结点中存储的数值
        printf("%d", p->data);
    }
}

void Mix(LinkList* La, LinkList* Lb, LinkList* Lc) {
    LinkList pa = (*La)->next;
    LinkList pb = (*Lb)->next;
    // pa和pb分别是链表La和Lb的工作指针,初始化为相应链表的第一个结点
    LinkList pc;
    *Lc = pc = *La; // 用La的头结点作为Lc的头结点
    while (pa && pb) {
        LinkList u;
        if (pa->data == pb->data) {
            // 交集并入结果表中
            pc->next = pa;
            pc = pa;
            pa = pa->next;
            u = pb;
            pb = pb->next;
            free(u);
        }
        else if (pa->data < pb->data) {
            u = pa;
            pa = pa->next;
            free(u);
        }
        else {
            u = pb;
            pb = pb->next;
            free(u);
        }
    }
    while (pa) {
        LinkList u = pa;
        pa = pa->next;
        free(u);
    } // 释放结点空间
    while (pb) {
        LinkList u = pb;
        pb = pb->next;
        free(u);
    }                // 释放结点空间
    pc->next = NULL; // 置链表尾标记。
    free(*Lb);       // 释放Lb的头结点
}
```





## 链接方向逆转

**编程实现：** 设计一个算法，将链表中所有结点的链接方向“原地”逆转，即要求仅利用原表的存储空间，换句话说，要求算法的空间复杂度为 O(1)。

**输入**

```
3（链表的元素个数）
1（链表的第 1 个元素）
2（链表的第 2 个元素）
3（链表的第 3 个元素）
```

**输出**

```
单链表逆置之后的形式:
321
```



#### 题目：

```c
#include <stdio.h>
#include <stdlib.h>
#include <malloc.h>

typedef struct node {
    char element[10];
    struct node *next;
} LinkList;

LinkList *Creat_LinkList (int n) {
    //创建链表
    
}

void print_LinkList (LinkList *p) {
    //输出链表
    
}

void reverse (LinkList *p) {
    //链表逆置
   
}

int main () {
    //根据题意，完成程序
    
    LinkList *L;
    int count; //设置单链表中元素个数
    printf("请输入单链表中元素个数: ");
    scanf("%d", &count);
    L = Creat_LinkList(count);
    printf("\n单链表逆置之前的形式:\n");
    print_LinkList(L);
    reverse(L); //单链表逆置
    printf("\n单链表逆置之后的形式:\n");
    print_LinkList(L);
}
```

#### 答案：

```c
LinkList* Creat_LinkList(int n) {
    LinkList* head = (LinkList*)malloc(sizeof(LinkList));
    head->next = NULL;
    LinkList* p = head;

    for (int i = 0; i < n; i++) {
        LinkList* newNode = (LinkList*)malloc(sizeof(LinkList));
        scanf("%s", newNode->element);
        newNode->next = NULL;
        p->next = newNode;
        p = p->next;
    }
    return head;
}

void print_LinkList(LinkList* p) {
    p = p->next;
    while (p) {
        printf("%s", p->element);
        p = p->next;
    }
    printf("\n");
}

void reverse(LinkList* p) {
    LinkList* prev = NULL;
    LinkList* current = p->next;
    LinkList* next = NULL;

    while (current != NULL) {
        next = current->next;
        current->next = prev;
        prev = current;
        current = next;
    }

    p->next = prev;
}
```

### 标准答案（不太好）

```C
LinkList* Creat_LinkList(int n) {
    //创建链表
    LinkList* p, * L, * q;
    int i;
    if ((L = (LinkList*)malloc(sizeof(LinkList))) == NULL) {
        exit(0); //创建头结点异常，正常运行程序并退出
    }
    L->element[0] = '\0'; //创建头结点
    L->next = NULL;       //创建空链表
    p = L;                //p指向头结点
    for (i = 0; i < n; i++) {
        if ((q = (LinkList*)malloc(sizeof(LinkList))) == NULL) {
            exit(0); //创建其余结点异常，并正常退出
        }
        p->next = q;
        printf("请输入第 %d 个元素: ", i + 1); //i的下标是从0开始的，得加1
        scanf("%s", q->element);            //%s代表字符串格式
        q->next = NULL;
        p = q;
    }
    return (L);
}

void print_LinkList(LinkList* p) {
    //输出链表
    while (p->next != NULL) {
        p = p->next; //跳过头结点输出下一个结点中存储的数值
        printf("%s", p->element);
    }
}

void reverse(LinkList* p) {
    //逆序输出单链表中的元素
    LinkList* x, * y, * z;
    x = p;
    y = p->next;
    while (y->next != NULL) {
        z = y->next;
        y->next = x;
        x = y;
        y = z;
    }
    y->next = x;
    p->next->next = NULL;
    p->next = y;
}

```





## 删除重复元素

已知线性表中的元素以值非递减有序排列，并以单链表作存储结构。试写一高效的算法，删除表中所有值相同的多余元素（使得操作后的线性表中所有元素的值均不相同），同时释放被删结点空间。

**输入**

```
3（链表的元素个数）
1（链表的第 1 个元素）
2（链表的第 2 个元素）
2（链表的第 3 个元素）
```

**输出**

```
删除相关元素后的链表为：12
```



#### 题目：

```c
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

void ListDelete_LSameNode(LinkList L) {
    // 删除链表中重复元素
    // 请在此处编写代码
    
    
}

int main() {
    // 创建链表
    LinkList L;

    // 输入数据
    int count;
    printf("请输入链表的元素个数：");
    scanf("%d", &count);
    printf("请输入链表的数据：\n");
    Create_LinkList(&L, count);
    ListDelete_LSameNode(L);
    printf("删除相关元素后的链表为：");
    print_LinkList(L);

    return 0;
}
```

#### 答案：

> 没有新东西，还是那些模式，`Create_LinkList`用了二重指针，然后还有第一题就出现的去重操作。

```c
#define _CRT_SECURE_NO_WARNINGS 1

#include <stdio.h>
#include <stdlib.h>

typedef struct LNode {
    int data;
    struct LNode* next;
} LNode, * LinkList;

void Create_LinkList(LinkList* L, int n) {

    *L = (LNode*)malloc(sizeof(LNode));
    (*L)->next = NULL;

    LinkList p = (*L);

    for (int i = 0; i < n; i++) {
        LinkList newNode = (LinkList)malloc(sizeof(LNode));
        scanf("%d", &(newNode->data));
        newNode->next = NULL;

        p->next = newNode;
        p = p->next;
    }
}

void print_LinkList(LinkList L) {
    LinkList temp = L->next;

    while (temp != NULL) {
        printf("%d ", temp->data);
        temp = temp->next;
    }
    printf("\n");
}

void ListDelete_LSameNode(LinkList L) {

    LinkList temp = L;

    while (temp && temp->next) {
        LinkList next = temp->next;
        if (temp->data == next->data) {
            temp->next = next->next;
            free(next);
        }
        else {
            temp = temp->next;
        }
    }
}

int main() {
    // 创建链表
    LinkList L;

    // 输入数据
    int count;
    printf("请输入链表的元素个数：");
    scanf("%d", &count);
    printf("请输入链表的数据：\n");
    Create_LinkList(&L, count);
    ListDelete_LSameNode(L);
    printf("删除相关元素后的链表为：");
    print_LinkList(L);

    return 0;
}
```

### 标准答案（不太好）

```C
void Create_LinkList(LinkList* L, int n) {
    // 创建链表
    *L = (LinkList)malloc(sizeof(LNode)); // 创建头结点
    (*L)->next = NULL;
    LinkList rear; // 尾指针
    rear = *L;      // 指向头结点
    for (int i = 0; i < n; i++) {
        LinkList p = (LinkList)malloc(sizeof(LNode)); // 给新结点创建空间
        p->next = NULL;
        printf("单链表中第%2d个元素是：", i + 1); // 下标是从0开始的，得加1
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

void ListDelete_LSameNode(LinkList L) {
    // 删除链表中重复元素
    LinkList p, q, prev;
    p = L;
    prev = p;
    p = p->next;
    while (p) {
        prev = p;
        p = p->next;
        if (p && p->data == prev->data) {
            prev->next = p->next;
            q = p;
            p = p->next;
            free(q); // 释放结点的内存空间
        }
    }
}
```





## 两个链表的差集

已知两个链表 A 和 B 分别表示两个集合，其元素递增排列。请设计算法求出两个集合 A 和 B 的差集（即仅由在 A 中出现而不在 B 中出现的元素所构成的集合），并以同样的形式存储，同时返回该集合的元素个数。

注意：两个链表一样时，输出为空，即什么也不输出。

**输入**

```
3,3（两个链表的元素个数）
1（第 1 个链表的第 1 个元素）
2（第 1 个链表的第 2 个元素）
3（第 1 个链表的第 3 个元素）
3（第 2 个链表的第 1 个元素）
4（第 2 个链表的第 2 个元素）
5（第 2 个链表的第 3 个元素）
```

**输出**

```
求差集后的的结果为：12
```



#### 题目：

```c
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

void Difference_Set(LinkList *La, LinkList *Lb, LinkList *Lc) {
    // 求两个链表的差集
    // 请在此处编写代码
    
    
}

int main() {
    // 创建2个链表
    LinkList La, Lb, Lc;

    // 输入数据
    int sum01, sum02;
    printf("两个链表的元素个数分别为：");
    scanf("%d,%d", &sum01, &sum02); // 输入以逗号分隔开
    printf("请输入第1个链表的数据：\n");
    Create_LinkList(&La, sum01);
    printf("请输入第2个链表的数据：\n");
    Create_LinkList(&Lb, sum02);

    Difference_Set(&La, &Lb, &Lc);
    printf("\n求差集后的结果为：");
    print_LinkList(La);

    return 0;
}
```

#### 答案：

> 这个题好像也是有问题，主函数里最后是用的`print_LinkList(La);`，不应该是用`Lc`带回来值，然后`print_LinkList(Lc);`吗。不管了，这儿我就用`Lc`了。
>
> Alpha提交的时候改一下主函数，改成`Lc`就行。（后来发现，`La`也行，都能过检查）

> 依旧是`Create_LinkList`的二重指针，`print_LinkList`，甚至直接用上一题的代码都行。
>
> 所以这一题的重点就是`Difference_Set`。
>
> 刚开始的时候忘了`Lc`没有初始化了，`Lc`在`Difference_Set`中要初始化一下。

> 这个题的答案用**标准答案**，因为标准答案确实是用`La`的。

```c
void Create_LinkList(LinkList* L, int n) {
    *L = (LNode*)malloc(sizeof(LNode));
    (*L)->next = NULL;

    LinkList p = *L;

    for (int i = 0; i < n; i++) {
        LinkList newNode = (LinkList)malloc(sizeof(LNode));
        scanf("%d", &(newNode->data));
        newNode->next = NULL;
        p->next = newNode;
        p = p->next;
    }
}

void print_LinkList(LinkList L) {
    LinkList temp = L->next;
    while (temp != NULL) {
        printf("%d", temp->data);
        temp = temp->next;
    }
    printf("\n");
}

void Difference_Set(LinkList* La, LinkList* Lb, LinkList* Lc) {
    LinkList pA = (*La)->next;
    LinkList pB = (*Lb)->next;

    *Lc = (LNode*)malloc(sizeof(LNode));
    (*Lc)->next = NULL;
    LinkList pC = *Lc;


    while (pA != NULL && pB != NULL) {
        if (pA->data < pB->data) {
            LinkList newNode = (LinkList)malloc(sizeof(LNode));
            newNode->data = pA->data;
            newNode->next = NULL;
            pC->next = newNode;
            pC = pC->next;
            pA = pA->next;
        }
        else if (pA->data > pB->data) {
            pB = pB->next;
        }
        else {
            pA = pA->next;
            pB = pB->next;
        }
    }

    while (pA != NULL) {
        LinkList newNode = (LinkList)malloc(sizeof(LNode));
        newNode->data = pA->data;
        newNode->next = NULL;
        pC->next = newNode;
        pC = pC->next;
        pA = pA->next;
    }
}
```

### 标准答案（Nice）

```C
void Create_LinkList(LinkList* L, int n) {
    *L = (LinkList)malloc(sizeof(LNode)); // 创建头结点
    (*L)->next = NULL;
    LinkList rear; // 尾指针
    rear = *L;     // 指向头结点
    for (int i = 0; i < n; i++) {
        LinkList p = (LinkList)malloc(sizeof(LNode)); // 给新结点创建空间
        p->next = NULL;
        printf("单链表中第%2d个元素是：", i + 1);  // 下标是从0开始的，得加1
        scanf("%d", &p->data);
        rear->next = p; // 新结点插入链表尾部
        rear = p;       // rear指向新的尾节点
    }
}

void print_LinkList(LinkList L) {
    LinkList p = L;
    while (p->next != NULL) {
        p = p->next;  // 跳过头结点输出下一个结点中存储的数值
        printf("%d", p->data);
    }
}

void Difference_Set(LinkList* La, LinkList* Lb, LinkList* Lc) {
    LinkList pa = (*La)->next;
    LinkList pb = (*Lb)->next;
    LinkList pc = *Lc = *La;
    while (pa && pb) {
        if (pa->data < pb->data) {
            pc = pa;
            pa = pa->next;
        }
        else if (pa->data > pb->data) {
            pb = pb->next;
        }
        else if (pa->data == pb->data) {
            pc->next = pa->next;
            free(pa);
            pa = pc->next;
        }
    }
}
```





## 分解链表

**编程实现：** 将一个带头结点的单链表 A 分解为两个具有相同结构的链表 B 和 C，其中 B 表的结点为 A 表中值小于零的结点，而 C 表的结点为 A 表中值大于零的结点(链表 A 中的元素为非零整数，要求 B、C 表利用 A 表的结点)。

注意：创建链表 A 时，如果输入 0，程序会将 0 在结果中删除。

**输入**

```
4（A链表的元素个数）
-2（A链表的第 1 个元素）
-1（A链表的第 2 个元素）
1（A链表的第 3 个元素）
2（A链表的第 4 个元素）
```

**输出**

```
B(存储负数)、C（存储正数）链表分别为：
-1 -2
2 1
```



#### 题目：

```c
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

void Split_List(LinkList *La, LinkList *Lb, LinkList *Lc) {
    // 将A链表分解成B、C两个链表
    // 请在此处编写代码
    
    
    
}

int main() {
    // 创建链表
    LinkList La, Lb, Lc;

    // 输入数据
    int sum;
    printf("A链表的元素个数为：");
    scanf("%d", &sum);
    printf("\n请输入A链表的数据：\n");
    Create_LinkList(&La, sum);

    Split_List(&La, &Lb, &Lc);
    printf("\nB(存储负数)、C（存储正数）链表分别为：\n");
    print_LinkList(Lb);
    printf("\n");
    print_LinkList(Lc);
    return 0;
}
```

#### 答案：

> 依旧是`Create_LinkList`的二重指针，`print_LinkList`，甚至直接用上一题的代码都行。
> 所以这一题的重点就是`Split_List`。
> 
>这部分和第一题“单链表的合并”一样，可以直接头插，也可以尾插+逆转

##### 方法一：直接头插

> 这多简单，尾插+逆转太繁琐了

```c
void Create_LinkList(LinkList* L, int n) {
    *L = (LNode*)malloc(sizeof(LNode));
    (*L)->next = NULL;

    LinkList p = *L;

    for (int i = 0; i < n; i++) {
        LinkList newNode = (LinkList)malloc(sizeof(LNode));
        scanf("%d", &(newNode->data));

        if ((newNode->data) != 0) {
            // 题目要求，如果输入 0，程序会将 0 在结果中删除
            newNode->next = NULL;
            p->next = newNode;
            p = p->next;
        }
    }
}

void print_LinkList(LinkList L) {
    LinkList temp = L->next;
    while (temp != NULL) {
        printf("%d ", temp->data);
        temp = temp->next;
    }
}

void Split_List(LinkList* La, LinkList* Lb, LinkList* Lc) {
    LinkList pA = (*La)->next;
    LinkList pB = NULL;
    LinkList pC = NULL;

    LinkList temp = (LinkList)malloc(sizeof(LNode));
    temp->next = NULL;

    *Lb = (LNode*)malloc(sizeof(LNode));
    (*Lb)->next = NULL;
    pB = *Lb;

    *Lc = (LNode*)malloc(sizeof(LNode));
    (*Lc)->next = NULL;
    pC = *Lc;

    while (pA != NULL) {
        if (pA->data < 0) {
            temp = pA->next;
            pA->next = pB->next;
            pB->next = pA;
            pA = temp;
        }
        else {
            temp = pA->next;
            pA->next = pC->next;
            pC->next = pA;
            pA = temp;
        }
    }
}
```



##### 方法二：尾插+逆转


```c
void Create_LinkList(LinkList* L, int n) {
    *L = (LNode*)malloc(sizeof(LNode));
    (*L)->next = NULL;

    LinkList p = *L;

    for ( int i = 0; i < n; i++) {
        LinkList newNode = (LinkList)malloc(sizeof(LNode));
        scanf("%d", &(newNode->data));

        if ((newNode->data) != 0) {
            // 题目要求，如果输入 0，程序会将 0 在结果中删除
            newNode->next = NULL;
            p->next = newNode;
            p = p->next;
        }
    }
}

void print_LinkList(LinkList L) {
    LinkList temp = L->next;
    while (temp != NULL) {
        printf("%d ", temp->data);
        temp = temp->next;
    }
}

void Split_List(LinkList* La, LinkList* Lb, LinkList* Lc) {
    LinkList pA = (*La)->next;
    LinkList pB = NULL;
    LinkList pC = NULL;

    *Lb = (LNode*)malloc(sizeof(LNode));
    (*Lb)->next = NULL;
    pB = *Lb;

    *Lc = (LNode*)malloc(sizeof(LNode));
    (*Lc)->next = NULL;
    pC = *Lc;

    while (pA != NULL) {
        if (pA->data < 0) {
            LinkList newNode = (LinkList)malloc(sizeof(LNode));
            newNode->data = pA->data;
            newNode->next = NULL;
            pB->next = newNode;
            pB = pB->next;
        }
        else {
            LinkList newNode = (LinkList)malloc(sizeof(LNode));
            newNode->data = pA->data;
            newNode->next = NULL;
            pC->next = newNode;
            pC = pC->next;
        }
        pA = pA->next;
    }

    // 逆转链表B
    pB = *Lb;
    LinkList prev = NULL;
    LinkList curr = pB->next;
    LinkList next;
    while (curr) {
        next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }
    pB->next = prev;

    // 逆转链表C
    pC = *Lc;
    prev = NULL;
    curr = pC->next;
    while (curr) {
        next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }
    pC->next = prev;
}
```

### 标准答案（Nice）

```c
void Create_LinkList(LinkList* L, int n) {
    *L = (LinkList)malloc(sizeof(LNode)); // 创建头结点
    (*L)->next = NULL;
    LinkList rear; // 尾指针
    rear = *L;     // 指向头结点
    for (int i = 0; i < n; i++) {
        LinkList p = (LinkList)malloc(sizeof(LNode)); // 给新结点创建空间
        p->next = NULL;
        printf("单链表中第%2d个元素是：", i + 1);  // 下标是从0开始的，得加1
        scanf("%d", &p->data);
        rear->next = p; // 新结点插入链表尾部
        rear = p;       // rear指向新的尾节点
    }
}

void print_LinkList(LinkList L) {
    LinkList p = L;
    while (p->next != NULL) {
        p = p->next;  // 跳过头结点输出下一个结点中存储的数值
        printf("%d ", p->data);
    }
}

void Split_List(LinkList* La, LinkList* Lb, LinkList* Lc) {
    // 将A链表分解成B、C两个链表
    
    LinkList pa = (*La)->next;
    *Lb = (LinkList)malloc(sizeof(LNode));
    *Lc = (LinkList)malloc(sizeof(LNode));
    (*Lc)->next = NULL;
    (*Lb)->next = NULL;  // 链表初始化，为空
    
    while (pa != NULL) {
        LinkList next = pa->next;
        if (pa->data < 0) {  // 把小于0的数放入B中
            pa->next = (*Lb)->next;
            (*Lb)->next = pa;
        }
        else if (pa->data > 0) {  // 把大于0的数放入C中
            pa->next = (*Lc)->next;
            (*Lc)->next = pa;
        }
        pa = next;  // 指向下一个待处理的点
    }
}
```

