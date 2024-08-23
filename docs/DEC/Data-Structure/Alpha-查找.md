# Alpha 查找



## 二分查找算法

请编写程序，完成`search`函数，实现二分查找算法，该函数返回关键字在数组中的位置，如果不存在则返回 `-1`。

### 示例输出

```
9
```

#### 题目

```c
//有序表的折半查找 
#include <stdio.h>

//请完成该函数 
//功能：实现有序表的折半查找功能
//返回值：如果找到返回数组下标（注意：数组下标从0开始），否则返回-1
//参数：a，有序数组
//参数：n，有序数组中元素的个数
//参数：key,查找的关键字 
int search(int a[], int n, int key)  
{
	//请在此处完成代码 
    
    

}

int main(){
    int a[11] = {5,13,19,21,37,56,64,75,80,88,92};
	int n = 11;
	int key = 88;
    int pos = search(a,n,key);
    printf("%d",pos);
    return 0;
} 
```

### 标准答案

```c
int search(int a[], int n, int key)  
{
    int low = 0;
	int high = n-1;
	int mid;
    while (low <= high)
    {
        mid = (low+high)/2;
        if (key == a[mid])
        	return mid;
		else if (key > a[mid])
            low = mid+1;
        else
            high = mid-1;
    }
    return -1; 
}
```





## 折半查找的递归算法

编写一个程序，实现折半查找的递归算法。程序从用户获取一个有序整数数组和一个要查找的整数。完善函数`BinSrch`使用递归实现折半查找算法，查找输入整数在有序数组中的位置。最后输出找到的元素的下标，如果找不到则输出 -1。

### 示例 1

**输入**

```
3
```

**输出**

```
2
```

### 示例 2

**输入**

```
10
```

**输出**

```
-1
```

#### 题目

```c
#include <stdio.h>

int BinSrch (int str[], int key, int low, int high) {
    //折半查找递归算法
    //请在此处编写代码
    
}

int main () {
    int str[] = { 1,2,3,4,5,6,7,8,9 };
    int index;
    printf("请输入查找的数字[找到返回下标，否则返回-1]：");
    scanf("%d", &index);
    index = BinSrch(str, index, 0, 8);
    printf("%d\n", index);
}
```

### 标准答案

> 我有个问题，为什么`mid`初始化不是`(low+high)/2`呢？不过都能通过检查。
>
> 另外，最后面的`return -1`也是可有可无，不会执行到那里的。

```c
int BinSrch (int str[], int key, int low, int high) {
    int mid = (low + high);
    if (low > high) {
        return -1;
    }
    if (key == str[mid]) {
        return mid;
    }
    else if (key > str[mid]) {
        return BinSrch(str, key, mid + 1, high);
    }
    else {
        return BinSrch(str, key, low, mid - 1);
    }
    
    return -1;
}
```





## 判别给定二叉树是否为二叉排序树

给定一棵二叉树，判断它是否为二叉排序树（BST）。二叉排序树是一种特殊的二叉树，对于树中的每个结点，其左子树的所有结点的值都小于该结点的值，而右子树的所有结点的值都大于该结点的值。请实现一个程序，输入一棵二叉树的先序遍历结果，判断该二叉树是否是二叉排序树。

### 示例

**输入**

```
请以先序遍历的的方式输入第一棵树（#表示结点没有子树）：532##4##76##8##
```

**输出**

```
该二叉树是二叉排序树！
```

#### 题目

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct BiTNode {
    char data;
    struct BiTNode *lchild, *rchild;
} BiTNode, *Bitree;

void Creat_Bitree(Bitree *T) {
    //请在此处编写代码
    
    
    
}

int flag = 1;      // 判断父结点的值是否大于孩子结点的值
int father_value = 0; // 用于存储中序遍历时父节点的值

int Isbstree(Bitree T) {
    //请在此处编写代码
    
    
    
}

int main() {
    Bitree T = NULL;
    printf("请以先序遍历的方式输入第一棵树（#表示结点没有子树）：");
    Creat_Bitree(&T);
    if (Isbstree(T))
        printf("\n该二叉树是二叉排序树！\n");
    else
        printf("\n该二叉树不是二叉排序树！\n");

    return 0;
}
```

#### 答案

> **方法一：**中序遍历，看是否从小到大排序，用`flag`记录值，如果值为`1`则是二叉排序树，反之则否
>
> **缺点：**需要遍历完整个二叉树才能的值是否是二叉排序树

> 这个和标准答案差不多

```c
int flag = 1;      // 判断父结点的值是否大于孩子结点的值
int father_value = NULL; // 用于存储中序遍历时父节点的值

int Isbstree(Bitree bt){
    if (bt != 0) {
        Isbstree(bt->lchild);

        if (father_value > bt->data) {
            flag = 0;
        }

        father_value = bt->data;

        Isbstree(bt->rchild);
    }
    return flag;
}
```
> **方法二：**先遍历根，再遍历所有的左子树，最后再遍历所有的右子树（不是前序遍历）
>
> 这种也挺有意思的。

```c
int Isbstree(Bitree T) { //返回1表示为二叉排序树，返回0表示不是 
    int flag1 = 0, flag2 = 0;

    if (T->lchild != NULL)
    {
        flag1 = 1;
        if (T->lchild->data > T->data) {
            return 0;
        }
    }
    if (flag1 == 1 && Isbstree(T->lchild) == 0) {
        return 0;    //这里flag1的作用是防止无限遍历左子树，flag2同理
        // 也就是如果有一个return 0了，那就一路向上return 0
    }

    if (T->rchild != NULL)
    {
        flag2 = 1;
        if (T->rchild->data < T->data) {
            return 0;
        }
    }
    if (flag2 == 1 && Isbstree(T->rchild) == 0) {
        return 0;
    }

    return 1;
}
```

### 标准答案（考试用这个）

> **二叉排序树**
>
> 1. 若它的左子树不空，则左子树上所有结点的值均小于根结点的值；
> 2. 若它的右子树不空，则右子树上所有结点的值均大于根结点的值；
> 3. 它的左、右子树也都分别是二叉排序树。

> **解题思路：**
>
> 1. 二叉排序树中序遍历所得结点值为增序；
> 2. 在遍历中将当前遍历结点与其前驱结点的值进行比较；
> 3. 设全局指针变量`pre`（初值为`null`）和`flag`（初值为`true`）。

```c
void Creat_Bitree(Bitree *T) {
    char ch;
    ch = getchar();
    if (ch == '#')
        *T = NULL;
    else {
        *T = (Bitree)malloc(sizeof(BiTNode));
        (*T)->data = ch;
        Creat_Bitree(&((*T)->lchild));
        Creat_Bitree(&((*T)->rchild));
    }
}

int flag = 1;      // 判断父结点的值是否大于孩子结点的值
int father_value = 0; // 用于存储中序遍历时父节点的值

int Isbstree(Bitree T) {
    if (T->lchild && flag) {
        Isbstree(T->lchild);
    }
        
    if (T->data < father_value) {
        flag = 0; // 左子树值大于父结点值
    }
       
    father_value = T->data; // 更新父结点的值\

    if (T->rchild && flag) {
        Isbstree(T->rchild);
    }
        
    return flag;
}
```





## 编写算法从小到大输出二叉排序树中所有数据值>=x的结点值

已知二叉排序树采用二叉链表存储结构，根结点的指针为`T`，链结点的结构为`(lchild, data, rchild)`，其中`lchild`，`rchild`分别指向该结点左、右孩子的指针，`data`域存放结点的数据信息。请编写算法，从小到大输出二叉排序树中所有`数据值 ≥ x`的结点的数据。要求先找到第一个满足条件的结点后，再依次输出其他满足条件的结点。

### 示例

**要求**

```
用 -1 表示无孩子结点
```

**输入**

```
请输入结点值：5
请输入结点值：3
请输入结点值：-1
请输入结点值：-1
请输入结点值：7
请输入结点值：-1
请输入结点值：-1
请输入x的值：4
```

**输出**

```
大于等于x的结点有：5 7
```

#### 题目

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct BiTNode {
    int data;
    struct BiTNode *lchild, *rchild;
} BiTNode, *Bitree;

void Creat_Bitree(Bitree *T) {
    //创建二叉树
    //请在此处编写代码
    
    
    
}

void Print_Node(Bitree T) {
    //中序输出
    //请在此处编写代码
    
    
    
}

void Print_All_x(Bitree T, int x) {
    //输出二叉排序树中大于等于x的结点值
    //请在此处编写代码
    
    
    
}

int main() {
    Bitree T = NULL;
    int x;
    Creat_Bitree(&T);
    printf("请输入x的值：");
    scanf("%d", &x);
    printf("大于等于x的结点有：");
    Print_All_x(T, x);
    return 0;
}
```

### 标准答案

> 很有意思，多看几遍理解理解

```c
void Creat_Bitree(Bitree* T) {
    int node;
    //printf("请输入结点值：");
    scanf("%d", &node);

    if (node == -1) {
        return;
    }

    *T = (Bitree)malloc(sizeof(BiTNode));
    (*T)->data = node;
    //(*T)->lchild = NULL;
    //(*T)->rchild = NULL;

    Creat_Bitree(&((*T)->lchild));
    Creat_Bitree(&((*T)->rchild));
}

void Print_Node(Bitree T) {
    if (T) {
        Print_Node(T->lchild);
        printf("%d ", T->data);
        Print_Node(T->rchild);
    }
}

void Print_All_x(Bitree T, int x) {
    // 定义指针p指向二叉树根节点
    Bitree p = T;
    // 如果根节点不为空
    if (p) {
        // 当节点值小于x时，沿右子树遍历
        while (p && p->data < x) {
            p = p->rchild;
        } // 直到找到第一个大于等于x的节点。

        // 以p为新的根节点
        T = p;

        // 如果p不为空
        if (p) {
            // 定义指针q指向p
            Bitree q = p;
            // 将p指向其左子树
            p = p->lchild;
            // 整个过程都不需要管右子树，因为右子树肯定>=x

            // 当节点值大于等于x时，沿左子树遍历
            while (p && p->data >= x) {
                q = p;
                p = p->lchild;
            }

            // 如果p不为空，将q的左子树置为空
            if (p) {
                q->lchild = NULL;
            }

            // 打印输出以T为根节点的子树
            Print_Node(T);
        }
    }
}
```





## 求平衡二叉树的高度

用户按照先序遍历的方式逐个输入二叉树的结点值，其中使用字符’#'表示结点没有子树。编写程序计算并输出该二叉树的高度。为了判断平衡性，每个结点的data字段用于表示平衡因子，当data小于0时，遍历右子树，否则遍历左子树。

### 示例

**输入**

```
请以先序遍历的的方式输入第一棵树（#表示结点没有子树）：532##4##76##8##
```

**输出**

```
该平衡二叉树的高度是：3
```

#### 题目

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct BiTNode {
    char data;
    struct BiTNode *lchild, *rchild;
} BiTNode, *Bitree;

void Creat_Bitree(Bitree *T) {
    //以先序遍历的方式创建二叉树 
    //请在此处编写代码
    
    
}

int Tree_Height(Bitree T) {
    // 求平衡二叉树的高度
    //请在此处编写代码
    
    
}

int main() {
    Bitree T = NULL;
    printf("请以先序遍历的方式输入第一棵树（#表示结点没有子树）：");
    Creat_Bitree(&T);
    printf("\n该平衡二叉树的高度是：%d", Tree_Height(T));

    return 0;
}
```

#### 答案

```c
int Tree_Height(Bitree T) {
    if (T == NULL) {
        return 0;
    } else {
        int left_height = Tree_Height(T->lchild);
        int right_height = Tree_Height(T->rchild);
        return (left_height > right_height) ? (left_height + 1) : (right_height + 1);
    }
}
```

### 标准答案（不正确）

> **解题思路：**根结点的层次为1，直到层数最大的叶子结点，是平衡二叉树的高度。当结点的平衡因子为0时，任选左右一分枝向下查找，若平衡因子等于1沿左遍历，若平衡因子等于-1沿右遍历。

> 上边是`Alpha`平台给出的解析，我认为不能说不太正确，只能说一派胡言。在这个平衡二叉树的结合中，`data`是元素数据，和这里的`1`、`-1`有什么关系。我不知道为什么能通过`Alpha`检查。

```c
void Creat_Bitree(Bitree *T) {
    char ch;
    ch = getchar();
    if (ch == '#')
        *T = NULL;
    else {
        *T = (Bitree)malloc(sizeof(BiTNode));
        (*T)->data = ch;
        Creat_Bitree(&((*T)->lchild));
        Creat_Bitree(&((*T)->rchild));
    }
}

int Tree_Height(Bitree T) {
    Bitree p = T;
    int height = 0;

    while (p) {
        height++;
        if (p->data < 0) {
            p = p->rchild;
        }
        else {
            p = p->lchild;
        }
    }
    return height;
}
```





## 二叉树中插入新节点

已知非空二叉排序树`T`的结点形式为`(lchild, data, rchild)`，在树中查找值为`X`的结点，若找到，则输出原来的二叉排序树，否则，作为一个新结点插入树中，插入后仍为二叉排序树，写出其非递归算法。

### 示例

**要求**

```
1、用 -1 表示无孩子结点
2、二叉排序树非空
3、使用非递归算法
```

**输入**

```
请输入结点值：5
请输入结点值：-1
请输入结点值：-1
请输入x的值：3
```

**输出**

```
插入数据后新的二叉排序树为：3 5
```

#### 题目

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct BiTNode {
    int data;
    struct BiTNode *lchild, *rchild;
} BiTNode, *Bitree;

void Creat_Bitree(Bitree *T) {
    //创建二叉树
    //请在此处编写代码
    
    
    
}

void Print_Node(Bitree T) {
    //中序遍历的方式输出
    //请在此处编写代码
    
    
    
}

void Search_Node(Bitree *T, int x) {
    //查找二叉排序树中的指定结点 
    //请在此处编写代码
    
    
    
}

int main() {
    Bitree T = NULL;
    int x;
    Creat_Bitree(&T);
    printf("请输入x的值：");
    scanf("%d", &x);
    Search_Node(&T, x);
    printf("插入数据后新的二叉排序树为：");
    Print_Node(T);
    return 0;
}
```

### 标准答案（不太正确）

> 解题思路：二叉排序树中利用其性质，先和根结点比较，如果等于根结点，则输出原二叉树，如果大于根结点，就遍历其右孩子，以此类推，如果小于根结点，就遍历其左孩子，以此类推。找到最终位置，插入二叉树中。

```c
void Creat_Bitree(Bitree* T) {
        int num;
        //printf("请输入结点值：");
        scanf("%d", &num);
    
        if (num == -1) {
                    return;
        }
    
        *T = (Bitree)malloc(sizeof(BiTNode));
        (*T)->data = num;
        //(*T)->lchild = NULL;
        //(*T)->rchild = NULL;
    
        Creat_Bitree(&((*T)->lchild));
        Creat_Bitree(&((*T)->rchild));
}

void Print_Node(Bitree T) {
    if (T) {
        Print_Node(T->lchild);
        printf("%d ", T->data);
        Print_Node(T->rchild);
    }
}

void Search_Node(Bitree *T, int x) {
    Bitree p, q, s;
    p = (Bitree)malloc(sizeof(BiTNode));
    p->data = x;
    p->lchild = p->rchild = NULL;
    if (!(*T)) {
        *T = p;
        exit(0);
    }
    s = NULL;
    q = *T;
    while (q) {
        s = q;
        if (q->data > x)
            q = q->lchild;
        else
            q = q->rchild;
    }
    if (s->data > x)
        s->lchild = p;
    else
        s->rchild = p;
}
```

### 改进答案

> 要注意`exit`和`return`的区别，然后就是原函数没有针对“树中有要插入的节点”进行操作

```c
void Creat_Bitree(Bitree* T) {
        int num;
        //printf("请输入结点值：");
        scanf("%d", &num);
    
        if (num == -1) {
                    return;
        }
    
        *T = (Bitree)malloc(sizeof(BiTNode));
        (*T)->data = num;
        //(*T)->lchild = NULL;
        //(*T)->rchild = NULL;
    
        Creat_Bitree(&((*T)->lchild));
        Creat_Bitree(&((*T)->rchild));
}

void Print_Node(Bitree T) {
    if (T) {
        Print_Node(T->lchild);
        printf("%d ", T->data);
        Print_Node(T->rchild);
    }
}

void Search_Node(Bitree* T, int x) {
    Bitree p, q, s;

    // 创建一个新节点，并将值 x 赋给它
    p = (Bitree)malloc(sizeof(BiTNode));
    p->data = x;
    p->lchild = p->rchild = NULL;

    // 若树为空，则将新节点作为树的根节点
    if (!(*T)) {
        *T = p;
        return;
    }

    // 初始化 s 为空，q 指向树的根节点
    s = NULL;
    q = *T;

    // 在树中查找新节点应该插入的位置
    while (q) {
        s = q; // s 是前驱节点
        if (q->data > x) {
            q = q->lchild;
        }
        else if (q->data < x) {
            q = q->rchild;
        }
        else {
            free(p);
            return; // 本身就有这个值
        }
    } // 注意 while 条件

    // 根据新节点的值与最后访问到的节点的值的大小关系，确定将新节点插入为左子节点还是右子节点
    if (s->data > x) {
        s->lchild = p;
    }
    else {
        s->rchild = p;
    }
}
```





## 编写从二叉排序树中删除一个关键字的算法

用户通过先序遍历的方式逐个输入一棵二叉树的结点值，其中使用特殊值`-1`表示结点没有子树。编写程序实现以下功能：

- 1、根据用户输入的**先序遍历**结果创建一棵二叉树。
- 2、用户输入一个特定的结点值，程序删除该值对应的结点，并保持二叉树的结构。
- 3、以**中序遍历**的方式输出删除指定结点后的二叉树。

### 示例

**输入**

```
请以先序遍历的的方式输入一棵树（-2 表示结点没有子树）：
请输入结点值：5
请输入结点值：3
请输入结点值：-1
请输入结点值：-1
请输入结点值：6
请输入结点值：-1
请输入结点值：-1
请输入要删除的结点值：3
```

**输出**

```
删除指定结点之后的二叉树以中序遍历的方式输出，结果是：5 6
```

#### 题目

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct BiTNode {
    int data;
    struct BiTNode *lchild, *rchild;
} BiTNode, *Bitree;

void Creat_Bitree(Bitree *T) {
    // 创建二叉树
    //请在此处编写代码
    
    
    
}

void Delete_Node(Bitree *T, int x) {
     // 删除指定结点
    //请在此处编写代码
    
    
    
}

void Print_Bitree(Bitree T) {
    // 以中序遍历的方式输出
    //请在此处编写代码
    
    
    
}

int main() {
    Bitree T = NULL;
    int x;
    printf("请以先序遍历的方式输入一棵树（-1 表示结点没有子树）：\n");
    Creat_Bitree(&T);
    printf("请输入要删除的结点值：");
    scanf("%d", &x);
    printf("删除指定结点之后的二叉树以中序遍历的方式输出，结果是：");
    Delete_Node(&T, x);
    Print_Bitree(T);
    return 0;
}
```

### 标准答案

> 看看`PPT`就都懂了。多思考理解。

```c
void Creat_Bitree(Bitree* T) {
    int num;
    printf("请输入结点值：");
    scanf("%d", &num);

    if (num == -1) {
        return;
    }

    *T = (Bitree)malloc(sizeof(BiTNode));
    (*T)->data = num;
    //(*T)->lchild = NULL;
    //(*T)->rchild = NULL;

    Creat_Bitree(&((*T)->lchild));
    Creat_Bitree(&((*T)->rchild));
}

void Delete_Node(Bitree *T, int x) {
    Bitree p, del, q, s;
    p = *T;
    del = NULL;
    while (p) {
        if (p->data == x)
            break;
        del = p;
        if (p->data > x)
            p = p->lchild;
        else
            p = p->rchild;
    }
    if (!p)
        return;
    q = p;
    if (p->lchild && p->rchild) {
        s = p->lchild;
        while (s->rchild) {
            q = s;
            s = s->rchild;
        }
        p->data = s->data;
        if (q != p)
            q->rchild = s->lchild;
        else
            q->lchild = s->lchild;
        free(s);
        return;
    } else if (!p->rchild) {
        p = p->lchild;
    } else if (!p->lchild) {
        p = p->rchild;
    }
    if (!del)
        *T = p;
    else if (q == del->lchild)
        del->lchild = p;
    else
        del->rchild = p;
    free(q);
}

void Print_Bitree(Bitree T) {
    if (T) {
        Print_Bitree(T->lchild);
        printf("%d ", T->data);
        Print_Bitree(T->rchild);
    }
}
```

> 注释

```c
void Delete_Node(Bitree* T, int x) {
    // 定义变量
    Bitree p, temp, delprev, dell;
    // 如果待删除节点有两个子节点，则有俩节点可以理解为 del
    // 一个用来替换的，一个被替换的
    // 此函数中  delprev、del是指用来替换的
    // temp 是被替换的节点的前驱，p 是被替换的节点

    // 从根节点开始遍历二叉树
    p = *T;
    temp = NULL;

    while (p) {
        // 如果找到待删除节点，则跳出循环
        if (p->data == x) {
            break;
        }

        // 保存当前节点，并更新指针以便继续向下搜索
        temp = p; // 被删除节点的前驱节点
        if (p->data > x) {
            p = p->lchild;
        }
        else {
            p = p->rchild;
        }
    }

    // 如果未找到待删除节点，则直接返回
    if (!p) {
        return;
    }

    // 执行到这儿的时候，p是被删除的
    delprev = p;

    
    // 如果待删除节点有两个子节点
    if (p->lchild && p->rchild) {
        dell = p->lchild;

        // 找到待删除节点左子树中最大的节点
        while (dell->rchild) {
            delprev = dell; // delprev 是 del 的前驱
            dell = dell->rchild; // del: 中序遍历下，被删除节点的前驱节点做根
            // 也就是左子树最右下的节点
        }

        // 用左子树最大节点的值替换待删除节点的值
        p->data = dell->data;

        // 重新连接父节点和左子树
        // 连接 del 的左孩子就行，因为 del 是最右下的，肯定没有右孩子
        if (delprev != p) { // del 的左孩子，有右孩子
            delprev->rchild = dell->lchild;
        }
        else { // del 的左孩子，没有右孩子
            delprev->lchild = dell->lchild;
        }

        free(dell);
        return;
    }
    // 如果待删除节点只有一个左子节点
    else if (!p->rchild) { // 只有左
        p = p->lchild;
    }
    // 如果待删除节点只有一个右子节点
    else if (!p->lchild) { // 只有右
        p = p->rchild;
    }

    
    // 更新父节点指向待删除节点的子节点
    if (!temp) { // 树的根被删除
        *T = p;
    }
    else if (delprev == temp->lchild) {
        temp->lchild = p;
    }
    else {
        temp->rchild = p;
    }

    free(temp);
}
```

## 待完成

最后这个题的`Delete_Node()`函数中的最后一个操作“更新父节点指向待删除节点的子节点”，我并没有看懂。
