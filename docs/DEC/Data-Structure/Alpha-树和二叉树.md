# Alpha 数和二叉树

## 二叉树的建立

**根据二叉树的中序和后序序列，建立二叉树**

已知一个二叉树的中序序列和后序序列，完成`CreateBitree`函数，实现建立该二叉树的二叉链表存储结构的功能。

**示例输出**

```
A B D E C F G 
```



#### 题目

```c
//已知一个二叉树的中序序列和后序序列，完成CreateBitree函数，实现建立该二叉树的二叉链表存储结构的功能
#include<stdio.h>
#include<stdlib.h>
#include<string.h>

//二叉树的存储表示 
typedef char TElemType;
typedef struct BiTNode{
	TElemType data;
	struct BiTNode *lchild, * rchild;	//左右孩子指针 
}BiTNode, *BiTree;

 
//inorder和postorder是二叉树的中序序列和后序序列
//lowin,highin是中序序列第一和最后结点的下标
//lowpost,highpost 是后序序列第一和最后结点的下标
//返回：二叉树的根结点指针 
BiTree CreateBitree(TElemType inorder[], TElemType postorder[], int lowin, int highin, int lowpost, int highpost)
{
    //请完成此函数代码
	
}


//按前序递归遍历二叉树 
void preorder(BiTree t)
{
	if(t)
	{
		printf("%c ",t->data);
		if(t->lchild)
			preorder(t->lchild);
		if(t->rchild)
			preorder(t->rchild);
	}
}

int main()
{
	TElemType in[]="DBEAFCG";//中序 
	TElemType post[]="DEBFGCA";	//后序 
	BiTree t;
	t = CreateBitree(in, post, 0,strlen(in)-1,0,strlen(post)-1);
	preorder(t);
}
```

#### 答案

```c

```





## 二叉树的叶子结点

已知一个二叉树的中序序列和后序序列，请完成`CalLeafNum`函数，返回二叉树叶子结点的个数。



#### 题目

```c
//求二叉树中叶子节点的个数

#include<stdio.h>
#include<stdlib.h>
#include<string.h>

//二叉树的存储表示 
typedef char TElemType;
typedef struct BiTNode{
	TElemType data;
	struct BiTNode *lchild, * rchild;	//左右孩子指针 
}BiTNode, *BiTree;


//计算叶结点的个数 
//返回：叶结点的个数 
int CalLeafNum(BiTree t) {
	//请在此填写代码 

}

BiTree CreateBitree(TElemType inorder[], TElemType postorder[], int lowin, int highin, int lowpost, int highpost)
{
	BiTree bt=(BiTree)malloc(sizeof(BiTNode));//申请结点
	bt->data=postorder[highpost];//后序遍历的最后一个结点是根结点
	int i = lowin;
	while(inorder[i]!=postorder[highpost]) //在中序序列中查找根结点
	   i++;
	if(i==lowin)
		bt->lchild=NULL;  //处理左子树
	else
		bt->lchild=CreateBitree(inorder,postorder,lowin,i-1,lowpost,lowpost+i-lowin-1);
	if(i==highin)
		bt->rchild=NULL;  //处理右子树
	else
		bt->rchild=CreateBitree(inorder,postorder,i+1,highin,lowpost+i-lowin,highpost-1);
	return bt;
}

int main()
{
	TElemType in[]="FDGBAHEC";//中序 
	TElemType post[]="FGDBHECA";	//后序 
	BiTree t;
	t = CreateBitree(in, post, 0,strlen(in)-1,0,strlen(post)-1);
	printf("%d",CalLeafNum(t));
}
```

#### 答案

```c

```







## 两棵二叉树是否相同

请编写一个程序，完成`sametree`函数，实现判断两棵二叉树是否是相同树的功能。每棵二叉树的结点包含字符数据，采用中序遍历和后序遍历的方式输入。如果两个二叉树相同时，返回1；不同时，则返回0。

**示例输出**

```
1
```



#### 题目

```c
//判断两棵二叉树是否相同
#include<stdio.h>
#include<stdlib.h>
#include <string.h>

//二叉树的存储表示 
typedef char TElemType;
typedef struct BiTNode{
	TElemType data;
	struct BiTNode *lchild, * rchild;	//左右孩子指针 
}BiTNode, *BiTree;

//请完成该函数，实现判断两棵二叉树是否是相同的树的功能 
//相同，返回1；不同，则返回0 
int sametree(BiTree t1, BiTree t2) {
    //在此输入代码
	
}

BiTree CreateBitree(TElemType inorder[], TElemType postorder[], int lowin, int highin, int lowpost, int highpost)
{
    int i = lowin;
	BiTree bt=(BiTree)malloc(sizeof(BiTNode));//申请结点
	bt->data=postorder[highpost];//后序遍历的最后一个结点是根结点
	
	while(inorder[i]!=postorder[highpost]) //在中序序列中查找根结点
	   i++;
	if(i==lowin)
		bt->lchild=NULL;  //处理左子树
	else
		bt->lchild=CreateBitree(inorder,postorder,lowin,i-1,lowpost,lowpost+i-lowin-1);
	if(i==highin)
		bt->rchild=NULL;  //处理右子树
	else
		bt->rchild=CreateBitree(inorder,postorder,i+1,highin,lowpost+i-lowin,highpost-1);
	return bt;
}

int main()
{
    BiTree t1;
    BiTree t2;
	TElemType in[]="DBEAFCG";//中序 
	TElemType post[]="DEBFGCA";	//后序 
	t1 = CreateBitree(in, post, 0,strlen(in)-1,0,strlen(post)-1);//建立第一个二叉树
	TElemType in2[]="DBEAFCG";//中序 
	TElemType post2[]="DEBFGCA";	//后序 
	t2 = CreateBitree(in2, post2, 0,strlen(in2)-1,0,strlen(post2)-1);//建立第二个二叉树
	printf("%d",sametree(t1,t2));
}
```

#### 答案

```c

```





## 判别两棵二叉树是否相等

请编写一个程序，实现判断两棵二叉树是否相同的功能。每棵二叉树采用先序遍历的方式输入，其中用’#'表示空结点。

### 示例

**输入**

```
请以先序遍历的的方式输入第一棵树（#表示结点没有子树）：a##

请以先序遍历的的方式输入第二棵树（#表示结点没有子树）：a##
```

**输出**

```
YES
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

int Judge_Same_Tree(Bitree T1, Bitree T2) {
    //请在此处编写代码
    
    
}

int main() {
    Bitree T1, T2;
    printf("请以先序遍历的方式输入第一棵树（#表示结点没有子树）：");
    Creat_Bitree(&T1);
    getchar();
    printf("\n请以先序遍历的方式输入第二棵树（#表示结点没有子树）：");
    Creat_Bitree(&T2);
    getchar();
    printf("\n");
    if (!Judge_Same_Tree(T1, T2))
        printf("NO");
    else
        printf("YES");

    return 0;
}
```

#### 答案

```c

```





## 求二叉树的深度

请完成`GetDepth`函数，实现计算二叉树深度的功能，函数返回二叉树的深度。

**示例输出**

```
3
```



#### 题目

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct Dnode{
    int data;
    struct Dnode *lchild,*rchild;
}Dnode,*Dtree;

void create(Dtree *T){
    int ch;
    scanf("%d",&ch);
    
    if(ch==-1){
        *T=NULL;
    }else{
        (*T)=(Dtree)malloc(sizeof(Dnode));
        (*T)->data=ch;
        create(&(*T)->lchild);
        create(&(*T)->rchild);
    }
}

void midpri(Dtree T){
    if(T==NULL){
        return ;
    }
    midpri(T->lchild);
    printf("%d\n",T->data);
    
    midpri(T->rchild);
}


int main(){
    Dtree T = NULL;
    //int x[]={1,4,6,7,8,9,11};
    create(&T);
    midpri(T);

}
```

#### 答案

```c

```





## 线索二叉树

请编写一个程序，实现对中序线索二叉树的中序扫描。每个结点包含字符数据和指向左右子树的指针，以及左右线索标记。左线索标记 LTag 为 0 表示指向左子树，为 1 表示指向中序遍历的前驱结点。右线索标记 RTag 为 0 表示指向右子树，为 1 表示指向中序遍历的后继结点。

**示例输出**

```
H D I B J E A F C G 
```



#### 题目

```c
//已知一中序线索二叉树,写一算法完成对它的中序扫描
#include<stdio.h>
#include<stdlib.h>
#include<string.h>

//二叉树的二叉线索存储表示 
typedef enum {Link, Thread} PointerTag; //Link==0:指针，Thread==1：线索 
typedef char TElemType;
typedef struct BiThrNode{
	TElemType data;
	struct BiThrNode *lchild, * rchild;	//左右孩子指针 
	PointerTag LTag,RTag; 
}BiThrNode, *BiThrTree;

void visit(BiThrTree t)
{
	printf("%c ",t->data);
}
//t是指向中序全线索化头结点的指针，该算法实现对线索二叉树中序遍历
void InOrderThreat(BiThrTree t){
	BiThrTree p=t->lchild;  //p指向二叉树的根结点，当二叉树为空时，p指向t
    //请在此完成代码

}

//创建树
BiThrTree CreateTree(){
    //定义结构体对象
    BiThrTree head = NULL,t1 = NULL, t2 = NULL, t3 = NULL, t4 = NULL, t5 = NULL, t6 = NULL, t7 = NULL, t8 = NULL, t9 = NULL, t10 = NULL;
    head = (BiThrTree)malloc(sizeof(BiThrNode));
    memset(head, 0, sizeof(BiThrNode));
	t1 = (BiThrTree)malloc(sizeof(BiThrNode));
    memset(t1, 0, sizeof(BiThrNode));
    t2 = (BiThrTree)malloc(sizeof(BiThrNode));
    memset(t2, 0, sizeof(BiThrNode));
    t3 = (BiThrTree)malloc(sizeof(BiThrNode));
    memset(t3, 0, sizeof(BiThrNode));
    t4 = (BiThrTree)malloc(sizeof(BiThrNode));
    memset(t4, 0, sizeof(BiThrNode));
    t5 = (BiThrTree)malloc(sizeof(BiThrNode));
    memset(t5, 0, sizeof(BiThrNode));
    t6 = (BiThrTree)malloc(sizeof(BiThrNode));
    memset(t6, 0, sizeof(BiThrNode));
    t7 = (BiThrTree)malloc(sizeof(BiThrNode));
    memset(t7, 0, sizeof(BiThrNode));
    t8 = (BiThrTree)malloc(sizeof(BiThrNode));
    memset(t8, 0, sizeof(BiThrNode));
    t9 = (BiThrTree)malloc(sizeof(BiThrNode));
    memset(t9, 0, sizeof(BiThrNode));
    t10 = (BiThrTree)malloc(sizeof(BiThrNode));
    memset(t10, 0, sizeof(BiThrNode));
    t1->data = 'A';
    t2->data = 'B';
    t3->data = 'C';
    t4->data = 'D';
    t5->data = 'E';
    t6->data = 'F';
    t7->data = 'G';
    t8->data = 'H';
    t9->data = 'I';
    t10->data = 'J';

    head->lchild = t1;
    head->rchild = t7;
    head->LTag = Link;
    head->RTag = Thread;
    
    t1->lchild = t2;
    t1->rchild = t3;
    t1->LTag = 0;
    t1->RTag = 0;

    t2->lchild = t4;
    t2->rchild = t5;
	t2->LTag = 0;
    t2->RTag = 0;
    
    t3->lchild = t6;
    t3->rchild = t7;
	t3->LTag = 0;
    t3->RTag = 0;
    
    t4->lchild = t8;
    t4->rchild = t9;
	t4->LTag = 0;
    t4->RTag = 0;
    
    t5->lchild = t10;
    t5->rchild = t1;
	t5->LTag = 0;
    t5->RTag = 1;
    
    t6->lchild = t1;
    t6->rchild = t3;
    t6->LTag = 1;
    t6->RTag = 1;

    t7->lchild = t3;
    t7->rchild = head;
    t7->LTag = 1;
    t7->RTag = 1;

    t8->lchild = head;
    t8->rchild = t4;
    t8->LTag = 1;
    t8->RTag = 1;

    t9->lchild = t4;
    t9->rchild = t2;
    t9->LTag = 1;
    t9->RTag = 1;

	t10->lchild = t2;
    t10->rchild = t5;
    t10->LTag = 1;
    t10->RTag = 1;
    return head;
}

int main()
{
	BiThrTree t;
	t = CreateTree();
	InOrderThreat(t);
	return 0;
}

```

#### 答案

```c

```





## 求二叉树的最大宽度

请编写一个程序，实现计算一棵二叉树的最大宽度的功能。每棵二叉树采用先序遍历的方式输入，其中用’#'表示空结点。二叉树的最大宽度是指二叉树所有层中结点个数的最大值。

### 示例

**输入**

```
请以先序遍历的的方式输入第一棵树（#表示结点没有子树）：a##
```

**输出**

```
该二叉树的最大宽度是：1
```



#### 题目

```C
#include<stdio.h>
#include<stdlib.h>
#define MAX 100 //定义队列的最大容量

typedef struct BiTNode {
    //定义结构体
    char data;
    struct BiTNode *lchild, *rchild;
} BiTNode, *Bitree;

void Creat_Bitree(Bitree *T) {
    //以先序遍历的方式创建二叉树
    //请在此处编写代码
    
    
}

int Account_Width(Bitree T) {
    //求二叉树最大宽度
    //请在此处编写代码
    
    
}

int main() {
    Bitree T = NULL;
    printf("请以先序遍历的方式输入第一棵树（#表示结点没有子树）：");
    Creat_Bitree(&T);
    printf("\n");
    printf("该二叉树的最大宽度是：%d", Account_Width(T));
    return 0;
}
```

#### 答案

```C

```





## 编写二叉树的双序遍历算法

请编写一个程序，实现二叉树的双序遍历功能。每棵二叉树采用先序遍历的方式输入，其中用’#'表示空结点。双序遍历是指对于二叉树的每一个结点来说，先访问这个结点，再按双序遍历它的左子树，然后再一次访问这个结点，接下来按双序遍历它的右子树。

### 示例

**输入**

```
请以先序遍历的的方式输入一棵树（#表示结点没有子树）：ab##c##
```

**输出**

```
双序遍历二叉树的结果为：abac
```



#### 题目

```C
#include<stdio.h>
#include<stdlib.h>

typedef struct BiTNode {
    //定义结构体
    char data;
    struct BiTNode *lchild, *rchild;
} BiTNode, *Bitree;

void Creat_Bitree(Bitree *T) {
    //以先序遍历的方式创建二叉树
    //请在此处编写代码
    
    
}

void DoubleTraverse_Tree(Bitree T) {
    //双序遍历二叉树
    //请在此处编写代码
    
    
}

int main() {
    Bitree T = NULL;
    printf("请以先序遍历的方式输入一棵树（#表示结点没有子树）：");
    Creat_Bitree(&T);
    printf("双序遍历二叉树的结果为：");
    DoubleTraverse_Tree(T);
    return 0;
}

```

#### 答案

```c

```





## 输出二叉树中从每个叶子结点到根结点的路径

请编写一个程序，实现输出一棵二叉树中每个叶子结点到根结点的路径。每棵二叉树采用先序遍历的方式输入，其中用’#'表示空结点。

### 示例

**要求**

```
1、输出的每条路径换行输出
2、每条路径中的结点用一个空格符分开
```

**输入**

```
请以先序遍历的的方式输入一棵树（#表示结点没有子树）：ab##c##
```

**输出**

```
该二叉树中从每个叶子结点到根结点的路径为：
b a
c a
```



#### 题目

```C
#include <stdio.h>
#include <stdlib.h>

#define MAX 100

typedef struct BiTNode {
    //定义结构体
    char data;
    struct BiTNode *lchild, *rchild;
} BiTNode, *Bitree;

void Creat_Bitree(Bitree *T) {
    //以先序遍历的方式创建二叉树
    //请在此处编写代码
    
    
}

void Node_Route(Bitree T, char *path, int path_length) {
    //二叉树中从每个叶子结点到根结点的路径
    //请在此处编写代码
    
    
}

int main() {
    Bitree T = NULL;
    char path[MAX];
    printf("请以先序遍历的方式输入一棵树（#表示结点没有子树）：");
    Creat_Bitree(&T);
    printf("该二叉树中从每个叶子结点到根结点的路径为：\n");
    Node_Route(T, path, 0);
    return 0;
}
```

#### 答案

```C

```





## 判别二叉树是不是完全二叉树

请编写一个程序，实现判断一棵二叉树是否是完全二叉树的功能。每棵二叉树采用先序遍历的方式输入，其中用’#'表示空结点。

### 示例

**输入**

```
请以先序遍历的的方式输入一棵树（#表示结点没有子树）：ab##c##
```

**输出**

```
YES
```



#### 题目

```C
#include <stdio.h>
#include <stdlib.h>

typedef struct BiTNode {
    //定义结构体
    char data;
    struct BiTNode *lchild, *rchild;
} BiTNode, *Bitree;

void Creat_Bitree(Bitree *T) {
    //以先序遍历的方式创建二叉树
    //请在此处编写代码
    
    
}

int Count_Nodes(Bitree T) {
    //计算二叉树的节点个数
    //请在此处编写代码
    
    
}

int Count_LeafNodes(Bitree T) {
    //计算二叉树的叶子节点个数
    //请在此处编写代码
    
    
}

int Judge_FullBiTree(Bitree T) {
    //判断二叉树是否为完全二叉树
    //请在此处编写代码
    
    
}

int main() {
    Bitree T = NULL;
    printf("请以先序遍历的方式输入一棵树（#表示结点没有子树）：");
    Creat_Bitree(&T);
    if (Judge_FullBiTree(T))
        printf("YES");
    else
        printf("NO");
    return 0;
}

```

#### 答案

```c

```

