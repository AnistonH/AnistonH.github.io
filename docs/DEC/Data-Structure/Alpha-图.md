# Alpha 图

## 深度优先遍历

给定图的邻接表表示和起始顶点索引，请编写程序，实现图的深度优先遍历函数`DFS`，并输出遍历的顶点序列。

### 示例输出

```
0 11 12 9 1 5 2 3 4 6 10 7 8
```

#### 题目

```C
//深度优先遍历 

#include<stdio.h>
#include<stdlib.h>
#include<string.h>

//图的邻接表的存储表示 
#define MAX_VERTEX_NUM 20
typedef int VertexType;
//表结点 
typedef struct ArcNode{
	int adjvex;		//该弧所指向顶点的位置 
	struct ArcNode*nextarc;	//指向下一条弧的指针 
}ArcNode;
//头结点 
typedef struct Vnode{
	VertexType data;	//顶点信息 
	ArcNode*firstarc;	//指向第一条依附该顶点的弧的指针 
	int visited;		//访问 
}Vnode,AdjList[MAX_VERTEX_NUM];

typedef struct{
	AdjList vertices;
	int vexnum,arcnum;	//图的当前顶点数和弧数 
	int kind;			//图的种类标志 
}ALGraph;

ALGraph *creategraph(int a[][4], int n)
{
	ALGraph *g=(ALGraph*)malloc(sizeof(ALGraph));
	g->vexnum=n;

	int i,j;
	for(i=0;i<n;i++)
	{
		g->vertices[i].data=i;
		g->vertices[i].visited=0;
		ArcNode *p = g->vertices[i].firstarc;
		for (j=0;j<4;j++)
		{
		    if(a[i][j]>=0)
		    {
		    	//printf("%d %d %d\n",i,j,a[i][j]);
		    	ArcNode *q = (ArcNode*)malloc(sizeof(ArcNode));
		    	q->adjvex=a[i][j];
		    	q->nextarc = NULL;
		    	//printf("q %d\n",q->adjvex);
		    	if(j==0){
		    		g->vertices[i].firstarc = q;
		    		//printf("g->vertices[i].firstarc %d\n",g->vertices[i].firstarc->adjvex);
				}else{
					p->nextarc = q;
				}
				p = q;
		    }else{
				break; 
			}
		    
		}
	}
	return g;
}

//请完成此处算法，实现图的深度优先遍历
//index表示搜索的顶点
//注：顶点编号从0开始 
void DFS(ALGraph *g,int index)
{    
    //请在此处填写代码
    
    
    
}

int main()
{
	int n = 13; //顶点数 
	int a[13][4] = {{11,5,2,1},{12,0,-1},{0,-1},{4,-1},{3,-1},{0,-1},{10,8,7,-1},{10,6,-1},{6,-1},{12,11,-1},{7,6,-1},{12,9,0,-1},{11,9,1,-1}};
	ALGraph *g=creategraph(a,n);
    DFS(g,0);
    DFS(g,3);
    DFS(g,6);
}
```

#### 答案

```c
void DFS(ALGraph* g, int index)
{
	// 获取当前顶点
	Vnode* node = &(g->vertices[index]); // 先找到这个点
	node->visited = 1;
	printf("%d ", node->data);

	// 遍历当前顶点的邻接点
	ArcNode* tmp = node->firstarc; // 先拿出这个边
	while (tmp != NULL) {
		int adjvex = tmp->adjvex; // 拿出这个边指向的顶点位置
		Vnode* adjnode = &(g->vertices[adjvex]); // 再拿出这个顶点

		// 若邻接点未被访问过，则递归访问该邻接点
		if (adjnode->visited == 0) {
			DFS(g, adjvex);
		}
		tmp = tmp->nextarc;
	}
}
```

### 标准答案（不太好）

```c
void DFS(ALGraph* g, int index)
{
	Vnode* node = &g->vertices[index];
	ArcNode* tmp = node->firstarc;

	if (node->visited == 0){
		printf("%d ", node->data);
		node->visited = 1;

		while (tmp != NULL) {
			DFS(g, tmp->adjvex);
			tmp = tmp->nextarc;
		}
	}
}
```





## 有向图顶点的入度

给定图的邻接表和一个顶点编号，请编写程序完成函数`GetIndegreeNum`，实现计算指定顶点的入度的功能。（注：顶点值编号从 0 开始，0～n-1）

### 示例输出

```
2
```

#### 题目

```c
//计算有向图某个顶点为的入度 （注：顶点值编号从0开始，0～n-1）
#include<stdio.h>
#include<stdlib.h>

//图的邻接表的存储表示 
#define MAX_VERTEX_NUM 20
typedef int VertexType;
//表结点 
typedef struct ArcNode{
	int adjvex;		//该弧所指向顶点的位置 
	struct ArcNode*nextarc;	//指向下一条弧的指针 
}ArcNode;
//头结点 
typedef struct Vnode{
	VertexType data;	//顶点信息 
	ArcNode*firstarc;	//指向第一条依附该顶点的弧的指针 
}Vnode,AdjList[MAX_VERTEX_NUM];

typedef struct{
	AdjList vertices;
	int vexnum,arcnum;	//图的当前顶点数和弧数 
	int kind;			//图的种类标志 
}ALGraph;


//建立有向图的邻接表存储结构
//n为顶点数
//返回建立的图的指针 
ALGraph *creategraph(int a[][4], int n)
{
	ALGraph *g=(ALGraph*)malloc(sizeof(ALGraph));
	g->vexnum=n;

	int i,j;
	for(i=0;i<n;i++)
	{
		g->vertices[i].data=i;
		ArcNode *p = g->vertices[i].firstarc;
		for (j=0;j<4;j++)
		{
		    if(a[i][j]>=0)
		    {
		    	//printf("%d %d %d\n",i,j,a[i][j]);
		    	ArcNode *q = (ArcNode*)malloc(sizeof(ArcNode));
		    	q->adjvex=a[i][j];
		    	q->nextarc = NULL;
		    	//printf("q %d\n",q->adjvex);
		    	if(j==0){
		    		g->vertices[i].firstarc = q;
		    		//printf("g->vertices[i].firstarc %d\n",g->vertices[i].firstarc->adjvex);
				}else{
					p->nextarc = q;
				}
				p = q;
		    }else{
				break; 
			}
		    
		}
	}
	return g;
}

//x:要计算入度的顶点编号，注：顶点编号从0开始
//返回值：该顶点的入度 
//请完成该函数，实现求某顶点的入度 
int GetIndegreeNum(ALGraph *g, int x){
 	//请在此处编写代码 
	
    
    
}

int main()
{
	int n = 4; //顶点数 
	int a[4][4] = {{2,1,-1},{3,-1},{3,-1},{0,-1}};
	ALGraph *g=creategraph(a,n);
	printf("%d",GetIndegreeNum(g, 3));
}  
```

### 标准答案

```c
int GetIndegreeNum(ALGraph* g, int x) {
	int indegree = 0; // 变量用于存储入度

	// 遍历图中的所有顶点
	for (int i = 0; i < g->vexnum; i++) {
		// 遍历每个顶点的邻接表
		ArcNode* p = g->vertices[i].firstarc; // 每个顶点指出去的第一条边
		while (p != NULL) {
			// 如果邻接顶点是x，则增加入度计数
			if (p->adjvex == x) {
				indegree++;
                //break;
			}
			p = p->nextarc;
		}
	}

	return indegree;
}
```





## 无向图邻接表存储结构删除指定的边

给定图的邻接表和两个顶点编号，编写程序，完成`DelEdge`函数，实现从无向图的邻接表中删除指定的边的功能。（注：顶点值编号从0开始，0～n-1）

### 示例输出

```
vertices:0 3 2 1 vertices:1 2 0 vertices:2 3 1 0 vertices:3 2 0
```

#### 题目

```c
//请完成DelEdge函数，实现从无向图的邻接表中删除某条边的功能。（注：顶点值编号从0开始，0～n-1）
#include<stdio.h>
#include<stdlib.h>

//图的邻接表的存储表示 
#define MAX_VERTEX_NUM 20
typedef int VertexType;
//表结点 
typedef struct ArcNode{
	int adjvex;		//该弧所指向顶点的位置 
	struct ArcNode*nextarc;	//指向下一条弧的指针 
}ArcNode;
//头结点 
typedef struct Vnode{
	VertexType data;	//顶点信息 
	ArcNode*firstarc;	//指向第一条依附该顶点的弧的指针 
}Vnode,AdjList[MAX_VERTEX_NUM];

typedef struct{
	AdjList vertices;
	int vexnum,arcnum;	//图的当前顶点数和弧数 
	int kind;			//图的种类标志 
}ALGraph;


//建立有向图的邻接表存储结构
//n为顶点数
//返回建立的图的指针 
ALGraph *creategraph(int a[][4], int n){
	ALGraph *g=(ALGraph*)malloc(sizeof(ALGraph));
	g->vexnum=n;

	int i;
	for(i=0;i<n;i++)
	{
		g->vertices[i].data=i;
		ArcNode *p = g->vertices[i].firstarc;
		for (j=0;j<4;j++)
		{
		    if(a[i][j]>=0)
		    {
		    	//printf("%d %d %d\n",i,j,a[i][j]);
		    	ArcNode *q = (ArcNode*)malloc(sizeof(ArcNode));
		    	q->adjvex=a[i][j];
		    	q->nextarc = NULL;
		    	//printf("q %d\n",q->adjvex);
		    	if(j==0){
		    		g->vertices[i].firstarc = q;
		    		//printf("g->vertices[i].firstarc %d\n",g->vertices[i].firstarc->adjvex);
				}else{
					p->nextarc = q;
				}
				p = q;
		    }else{
				break; 
			}
		    
		}
	}
	return g;
}

//打印图
void DisplayGraph(ALGraph *g){
    //printf("顶点:%d，弧:%d\n", g->vexnum,g->arcnum);
	int i,j;
    for ( i = 0; i < g->vexnum; i++)
    {
        printf("vertices:%d ", g->vertices[i].data);
        ArcNode* p = g->vertices[i].firstarc;
        while( p != NULL)
        {
        	printf("%d ", p->adjvex);
        	p = p->nextarc;
        }
        //printf("\n");
    }
}

//x,y为要删除边的两个顶点编号，注：顶点编号从0开始 
//请完成此函数，实现从无向图的邻接表中删除某条边的功能 
void DelEdge(ALGraph *g,int x, int y){
 	//请在此处完成该函数 
     
     
	
}

int main(){
	int n = 4; //顶点数 
	int a[4][4] = {{3,2,1,-1},{3,2,0,-1},{3,1,0,-1},{2,1,0,-1}};
	ALGraph *g=creategraph(a,n);
	DelEdge(g, 3, 1);
	DisplayGraph(g);
} 
```

#### 答案

> `creategraph()`函数有错误，没有定义变量`j`。加上就行了

```c
void DelEdge(ALGraph* g, int x, int y) {
    ArcNode* px = g->vertices[x].firstarc;
    ArcNode* py = g->vertices[y].firstarc;
    ArcNode* px_prev = NULL;
    ArcNode* py_prev = NULL;

    // 在顶点x的邻接表中查找边，并记录前一个结点
    while (px != NULL && px->adjvex != y) {
        px_prev = px;
        px = px->nextarc;
    }

    // 在顶点y的邻接表中查找边，并记录前一个结点
    while (py != NULL && py->adjvex != x) {
        py_prev = py;
        py = py->nextarc;
    }

    // 如果在两个邻接表中都找到相应的边，则从中删除边，并释放内存
    if (px != NULL && py != NULL) {
        // 从顶点x的邻接表中删除边
        if (px_prev == NULL) {
            g->vertices[x].firstarc = px->nextarc;
        }
        else {
            px_prev->nextarc = px->nextarc;
        }
        free(px);

        // 从顶点y的邻接表中删除边
        if (py_prev == NULL) {
            g->vertices[y].firstarc = py->nextarc;
        }
        else {
            py_prev->nextarc = py->nextarc;
        }
        free(py);

        // 边的数量减1
        g->arcnum--;
    }
}
```

### 标准答案（不太好）

> 其实和咱那个也一样

```c
void DelEdge(ALGraph *g,int x, int y)
{
	ArcNode* p = g->vertices[x].firstarc;
	ArcNode* pre = g->vertices[x].firstarc;
    if(p->adjvex == y){
    	g->vertices[x].firstarc = p->nextarc;
    	free(p);
	}else{
		while(p->nextarc != NULL){
			p = p->nextarc;
			if(p->adjvex == y){
        		pre->nextarc = p->nextarc;
        		free(p);
        		break;
        	}
        	pre = p;
		}
	}
	
	p = g->vertices[y].firstarc;
	pre = g->vertices[y].firstarc;
    if(p->adjvex == x){
    	g->vertices[y].firstarc = p->nextarc;
    	free(p);
	}else{
		while(p->nextarc != NULL){
			p = p->nextarc;
			if(p->adjvex == x){
        		pre->nextarc = p->nextarc;
        		free(p);
        		break;
        	}
        	pre = p;
		}
	}
}
```





## 普里姆算法

有一个带权无向图，图中的每条边都有一个权值。请编写程序，完成`prim`函数，实现利用普里姆算法构造该图的最小生成树，并计算最小生成树的权值之和。

### 示例输出

```
15
```

#### 题目

```c
#include<stdio.h>
#include<stdlib.h>

#define INF  0x7fffffff
#define MAX 100  

//请完成利用普里姆算法构造最小生成树，函数返回最小生成树的权值的和 
//参数：n，表示图的顶点数
//graph，表示图的邻接矩阵 
int prim(int graph[][MAX], int n)  
{  	
    //请在此处完成代码
    
    
    
}

int main()  
{  
	int edge[MAX][MAX]={{INF,INF,INF,INF,INF,INF,INF},\
	{INF,INF,6,1,5,INF,INF},\
	{INF,6,INF,5,INF,3,INF},\
	{INF,1,5,INF,5,6,4},\
	{INF,5,INF,5,INF,INF,2},\
	{INF,INF,INF,3,INF,6,6},\
	{INF,INF,INF,4,2,6,INF}};

    printf("%d",prim(edge, 6));  
    return 0;  
}  
```

### 标准答案

```C
int prim(int graph[][MAX], int n)
{
    int lowcost[MAX];  // 记录当前生成树到各顶点的最小权值
    int mst[MAX];  // 记录最小生成树的顶点
    int i, j, min, minid, sum = 0;  // 循环变量和累加总权值的变量

    // 初始化lowcost和mst数组
    for (i = 2; i <= n; i++) { // 下标从1开始
        lowcost[i] = graph[1][i];
        mst[i] = 1;
    }

    mst[1] = 0;  // 将起始顶点加入最小生成树
    for (i = 2; i <= n; i++) { // i用于控制外层
        min = INF;
        minid = 0;

        // 寻找lowcost中最小权值的顶点
        for (j = 2; j <= n; j++) { 
            // 更新最小权值和对应顶点
            if (lowcost[j] < min && lowcost[j] != 0) { // lowcost[j]>0 表示顶点还在图中
                min = lowcost[j];
                minid = j;
            }
        }

        // 将最小权值加入总权值
        sum += min;
        lowcost[minid] = 0;  // 将已加入最小生成树的顶点权值设为0

        for (j = 2; j <= n; j++) {
            // 更新生成树到各顶点的最小权值和对应顶点
            if (graph[minid][j] < lowcost[j]) {
                lowcost[j] = graph[minid][j];
                mst[j] = minid;
            }
        }
    }

    return sum;  // 返回最小生成树的总权值
}
```





## 最短路径（Floyd算法）

有一个带权有向图，图中的每条边都有一个权值。请编写程序，完成`Floyd`函数，实现利用弗洛伊德算法求解该图的最短路径。

### 示例输出

```
0 6 1 5 7 5 6 0 5 10 3 9 1 5 0 5 6 4 5 10 5 0 8 2 7 3 6 8 0 6 5 9 4 2 6 0
```

#### 题目

```c
//利用弗洛伊德算法求最短路径 
#include <stdio.h>
#define INF  1000

//请完成该函数，实现 利用弗洛伊德算法求最短路径
//参数：n，代表图的顶点个数
//      r,表示顶点路径的值，如，r[2][3],表示从顶点2到顶点3的最小路径的值 
void Floyd(int r[][7], int n)
{
	//请在此处完成代码 
    
    

}

int main(){
	int e[7][7]={{INF,INF,INF,INF,INF,INF,INF},\
	{INF,0,6,1,5,INF,INF},\
	{INF,6,0,5,INF,3,INF},\
	{INF,1,5,0,5,6,4},\
	{INF,5,INF,5,0,INF,2},\
	{INF,INF,3,6,INF,0,6},\
	{INF,INF,INF,4,2,6,0}};
	int n = 6;
	Floyd(e, n);
	
	//输出最终的结果
	int i,j;
	for(i=1;i<=n;i++)
	{
		for(j=1;j<=n;j++)
		{
			printf("%d ",e[i][j]);
		}
	}
	return 0;
}
```

### 标准答案

```C
void Floyd(int r[][7], int n) {
	int k, i, j;
	for (k = 1; k <= n; k++) // k是中间点
	{
		for (i = 1; i <= n; i++)
		{
			for (j = 1; j <= n; j++)
			{
				if (r[i][j] > r[i][k] + r[k][j]) {
					r[i][j] = r[i][k] + r[k][j];
				}
			}
		}
	}
}
```





## 计算无向图中各个顶点的度

请设计一个程序，实现无向图的度计算功能。给定一个无向图，使用邻接表表示，计算图中每个顶点的度。

### 示例

**要求**

```
1、顶点编号从0开始
2、输入输出规则参考下面的例子
```

**输入**

```
请输入图的顶点个数和边的个数：3,2

请输入第1条边（顶点从0开始）：0-1
请输入第2条边（顶点从0开始）：1-2
```

**输出**

```
该无向图中顶点0到顶点2的度依次为：1 2 1
```

#### 题目

```c
#include <stdio.h>
#include <stdlib.h>
#define Max_Vertex_Num 100

// 定义结构体
typedef struct ArcNode {
    int adjvex;
    struct ArcNode *nextarc; // 下一个节点
} ArcNode;

typedef struct VNode {
    int vertex;            // 头结点数据
    ArcNode *firstarc;     // 指向该顶点第一条边的指针
} VNode, Adjlist[Max_Vertex_Num]; // 头结点数组

typedef struct {
    Adjlist adjlist; // 邻接表数组，每个元素存储与该顶点关联的点
    int vexnum, arcnum; // 顶点数和边数
} ALGraph; // 邻接表

int Locate_Vex(ALGraph *G, int v) {
    // 确定输入边的位置（由顶点确定）

}

void Create_ALGraph(ALGraph *G) {
    // 创建无向图

}

void Account_Degree(ALGraph *G) {

}

int main() {
    ALGraph G;
    Create_ALGraph(&G); // 创建无向图
    Account_Degree(&G); // 计算度
    return 0;
}
```

### 标准答案

> 还算条理清晰

```c
int Locate_Vex(ALGraph* G, int v) {
    // 确定输入边的位置（由顶点确定）
    for (int i = 0; i < G->vexnum; i++) {
        if (G->adjlist[i].vertex == v) {
            return i;
        }
    }
}

void Create_ALGraph(ALGraph *G) {
    // 创建无向图
    ArcNode *p;
    int v1, v2, x; // 输入边的两个顶点
    
    printf("请输入图的顶点个数和边的个数：");
    scanf("%d,%d", &G->vexnum, &G->arcnum); // 用逗号分隔

    for (x = 0; x < G->vexnum; x++) {
        G->adjlist[x].vertex = x;
        G->adjlist[x].firstarc = NULL;
    } // 初始化
    
    x = 0;
    printf("\n");
    
    while (x < G->arcnum) {
        printf("请输入第%d条边（顶点从0开始）：", x + 1);
        scanf("%d-%d", &v1, &v2);
        x++;
        
        int i = Locate_Vex(G, v1);
        int j = Locate_Vex(G, v2);

        p = (ArcNode *)malloc(sizeof(ArcNode)); // 添加一个数据为j的边结点到i的表头
        p->adjvex = j;
        p->nextarc = G->adjlist[i].firstarc; // 可以理解为头插
        G->adjlist[i].firstarc = p;

        p = (ArcNode *)malloc(sizeof(ArcNode)); // 添加一个数据为i的边结点到j的表头
        p->adjvex = i;
        p->nextarc = G->adjlist[j].firstarc;
        G->adjlist[j].firstarc = p;
    }
}

void Account_Degree(ALGraph *G) {
    ArcNode *p;
    int i;
    printf("\n该无向图中顶点0到顶点%d的度依次为：", G->vexnum - 1);
    
    for (i = 0; i < G->vexnum; i++) {
        int degree = 0;
        p = G->adjlist[i].firstarc;
        while (p != NULL) {
            degree++;
            p = p->nextarc;
        }
        printf("%d ", degree);
    }
}
```

> **对`Create_ALGraph`函数中最后一部分的讲解**（以第一大块为例）
>
>   1. 使用`malloc`函数分配了内存以存储一个新的`ArcNode`类型的边结点。
>   2. 将这个边结点的`adjvex`属性设置为目标顶点`j`的值，表示这条边指向顶点`j`。
>   3. 将这个边结点的`nextarc`属性指向顶点`i`原本的第一条边，以保证新添加的边在链表中排在原有的边之前。
>   4. 将顶点`i`的`firstarc`指针指向新添加的边结点，完成了在邻接表中添加边的过程。
