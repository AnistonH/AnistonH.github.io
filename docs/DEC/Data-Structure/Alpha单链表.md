# Alpha 单链表

## 单链表的合并

> 涉及：单链表的合并  删除重复数据  逆转元素

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

**代码：**

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





## 合并递增有序链表

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

**代码：**

```c
#define _CRT_SECURE_NO_WARNINGS 1

#include <stdio.h>
#include <stdlib.h>

// 定义
typedef struct {
    int date;
} Date;

typedef struct LNode {
    Date elem;
    struct LNode* next;
} LNode, * LinkList;

void Create_LinkList(LinkList* L, int n) {
    *L = (LinkList)malloc(sizeof(LNode)); // 为头节点分配内存
    (*L)->next = NULL; // 初始化头节点的下一个指针为NULL

    LinkList p = *L; // 用于遍历链表的指针

    printf("请输入链表的数据:\n");
    for (int i = 0; i < n; i++) {
        p->next = (LinkList)malloc(sizeof(LNode)); // 为下一个节点分配内存
        p = p->next; // 移动到新创建的节点
        printf("请输入第 %d 个元素的值: ", i + 1);
        scanf("%d", &p->elem.date); // 读取当前节点的数据
    }

    p->next = NULL; // 将最后一个节点的下一个指针设为NULL
}


void MergeList(LinkList La, LinkList Lb, LinkList* Lc) {
    //合并链表La和Lb，合并后的新表使用头指针Lc指向
    //请在此处编写代码
    LinkList pa = La->next;  // 从La的第一个节点开始
    LinkList pb = Lb->next;  // 从Lb的第一个节点开始

    *Lc = La;  // 将Lc指向La的头节点
    LinkList pc = *Lc;  // 初始化一个指向Lc头节点的指针

    while (pa && pb) {
        if (pa->elem.date <= pb->elem.date) {
            pc->next = pa;  // 将来自La的节点链接到Lc
            pa = pa->next;  // 移动到La的下一个节点
        }
        else {
            pc->next = pb;  // 将来自Lb的节点链接到Lc
            pb = pb->next;  // 移动到Lb的下一个节点
        }
        pc = pc->next;  // 移动到Lc的下一个节点
    }

    // 如果La或Lb中还有剩余的节点，将它们链接到Lc中
    pc->next = pa ? pa : pb;


    // 删除重复元素
    LinkList pC = *Lc;

    while (pC && pC->next) {
        LinkList next = pC->next;
        if (pC->elem.date == next->elem.date) {
            pC->next = next->next;
            free(next);
        }
        else {
            pC = pC->next;
        }
    }
}

// 输出数据
void print_LinkList(LinkList L) {
    //请在此处编写代码
    LinkList p = L->next; // 从第一个节点开始
    while (p) {
        printf("%d ", p->elem.date); // 打印数据
        p = p->next; // 移动到下一个节点
    }
    printf("\n");
}

int main() {
    // 创建2个链表
    LinkList La, Lb, Lc;

    // 输入数据
    int sum01, sum02;
    printf("两个链表的元素个数分别为: ");
    scanf("%d %d", &sum01, &sum02); // 输入以英文逗号分隔开
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

