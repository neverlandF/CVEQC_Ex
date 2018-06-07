#include<stdio.h>
main()
{
	int c;
	while ((c = getchar()) != EOF)
		putchar(c);

}

/*== 3.1 线性结构 示例代码 ==========================================*/
/*===================================================================*/
/*-- 3.1.1 线性表 ---------------------------------------------------*/

// 单链表结点类型
typedef struct node {
	int data;			// 结点的数据域，此处假设为整型
	struct node *next;	// 结点的指针域
} NODE, *LinkList;

// -- 单链表的查找运算 --
//	在表中查找第k个元素，若找到，返回该元素结点的指针
//	否则，返回空指针NULL
LinkList Find_List(LinkList L, int k)	// L为带头结点单链表的头指针
{
	LinkList p;
	int i;
	i = 1;			// 计数器
	p = L->next;	// 初始时另p指向第一个结点
	while (p && i < k) {	// 顺指针链向后查找，直到p指向
		p = p->next;		//	  第k个元素结点或p为空指针
		i++;
	}
	if (p && i == k) return p;	// 存在第k个元素且p指向它
	return NULL;				// 第k个元素不存在，返回空指针
}/*Find_List*/

 // -- 单链表的插入运算 --
 //	将元素newElem插入表中第k个元素之前，若成功返回0，否则返回-1
 //	该插入操作等同于将元素插入在第k-1个元素之后
int Insert_List(LinkList L, int k, int newElem)
{
	LinkList p, s;			// 临时指针
	if (k == 1) p = L;		// 如元素要插入到第1个元素前
	else p = Find_List(L, k - 1);	// 查找表中的第k-1个元素并令指针指向它
	if (!p) return -1;				// 如表中不存在第k-1个元素，不满足要求
	s = (NODE*)malloc(sizeof(NODE));	// 创建新元素的结点空间
	if (!s) return -1;
	s->data = newElem;
	s->next = p->next;		// 插入元素
	p->next = s;
}/*Insert_List*/

 // -- 单链表的删除运算 --
 //	删除表中第k个元素，若成功返回0，否则返回-1
 //	该删除操作等同于将第k-1个元素的结点指针域指向第k+1个元素所在结点
int Delete_List(LinkList L, int k)
{
	LinkList p, q;			// 临时指针
	if (k == 1) p = L;		// 如删除的是第一个元素
	else p = Find_List(L, k - 1);	// 查找第k-1个元素并将p指向它
	if (!p || !p->next) return -1;	// 表中不存在第k个元素
	q = p->next;			// 删除元素
	p->next = q->next;
	free(q);
}/*Delete_List*/


/*-- 3.1.2 栈和队列 --------------------------------------------------*/

// 循环队列类型定义
#define MAXQSIZE 100
typedef struct {
	int *base;	// 队列存储空间，假设存放的是整数
	int front;	// 指示队头，称为队头指针
	int rear;	// 指示队尾，称为队尾指针
}SqQueue;

// 创建一个空的循环队列,容量为MAXQSIZE，若成功返回0，否则返回-1
int InitQueue(SqQueue *Q)
{
	Q->base = (int*)malloc(MAXQSIZE * sizeof(int));
	if (!Q->base) return -1;
	Q->front = 0;
	Q->rear = 0;
	return 0;
}/*InitQueue*/

// 元素入循环队列：元素e入队，成功返回0，否则返回-1
int EnQueue(SqQueue *Q, int e)
{
	if ((Q->rear + 1) % MAXQSIZE == Q->front) return -1;
	Q->base[Q->rear] = e;
	Q->rear = (Q->rear + 1) % MAXQSIZE;
	return 0;
}/*EnQueue*/

// 元素出循环队列
// 若队列不空，则删除队头元素，由参数e待会其值并返回0，否则返回-1
int DelQueue(SqQueue *Q, int *e)
{
	if (Q->rear == Q->front) return -1;
	*e = Q->base[Q->front];
	Q->front = (Q->front + 1) % MAXQSIZE;
	return 0;
}/*DelQueue*/


/*-- 3.1.3 串 --------------------------------------------------------*/

// 朴素的模式匹配算法
// 从主串第一位开始对比，如第1位开始和子串所有位都相等则结束比对返回匹配成功信息
// 如未与子串完全相等，在第n位有差异，则指针回退到主串第2位和子串第1位开始重新比对
// 查找并返回模式串T在主串S中从pos开始的位置（下标），若T不是S的子串，则返回-1
int Index(char S[], char T[], int pos)
{
	int i, j, slen, tlen;
	i = pos; j = 0;			// i,j分别用于指出主串和模式串字符的位置
	slen = strlen(S);		// 计算主串和模式串的长度
	tlen = strlen(T);
	while (i < slen&&j < tlen) {
		if (S[i] == T[j]) { i++; j++; }
		else {
			i = i - j + 1;	// 主串字符的指针回退
			j = 0;
		}
	}/*while*/
	if (j >= tlen) return i - tlen;		
	return -1;
}/*Index*/

// 改进的模式匹配算法（KMP算法）
// 从主串第一位开始对比，如第1位开始和子串所有位都相等则结束比对返回匹配成功信息
// 如未与子串完全相等，在第n位有差异，则指针滑动到主串第n位和子串第1位开始重新比对

// 实现匹配失败时指针滑动
void Get_next(char*p, int next[])
{
	int i, j, slen;
	slen = strlen(p);
	i = 0;
	next[0] = -1;
	j = -1;
	while (i < slen) {
		if(j==-1||p[i]==p[j]){
			++i;
			++j;
			next[i] = j;
		}
		else j = next[j];
	}/*while*/
}/*Get_next*/

// KMP算法，设模式串第一个字符下标为0
// 利用模式串p的next函数，求p在主串s中从第一个pos个字符开始的位置
int Index_KMP(char*s, char*p, int pos, int next[])
{
	int i, j, slen, plen;
	i = pos - 1;
	j = -1;
	slen = strlen(s);
	plen = strlen(p);
	while (i < slen&&j < plen) {
		if (j == -1 || s[i] == p[i]) {
			++i;
			++j;
		}
		else j = next[j];
	}/*while*/
}/*Index_KMP*/



/*== 3.3 树 示例代码 ================================================*/
/*===================================================================*/





/*-- 3.3.3 二叉树的遍历 ---------------------------------------------*/

// 二叉链表结点定义
typedef struct BiTnode {
	int data;
	struct BiTnode *lchild, *rchild;
}; BiTnode, *BiTree;

/*=====二叉树的先序遍历=====*/
void PreOrder(BiTree root) {
	if (root != NULL) {
		printf("%d", root->data);
		PreOrder(root->lchild);
		PreOrder(root->rchild);
	}
}

/*== 3.3.5 哈夫曼树和哈夫曼编码 示例代码 ============================*/
/*===================================================================*/
#define MAXLEAFUNM 50	/*最优二叉树中的最多叶子数目*/
typedef struct node
{
	char ch;		/*结点表示的字符，对于非叶子结点，此域不用*/
	int weight;		/*结点的权值*/
	int parent;		/*结点的父结点的下标*/
	int lchild, rchild;		/*结点左右孩子结点的下标，为0时表示无孩子*/
}HuffmanTree[2 * MAXLEAFUNM];
typedef char* HuffmanCode[MAXLEAFUNM + 1];

/*=====哈夫曼树构建 =====*/
void createHTree(HuffmanTree HT, char* c, int* w, int n)
{	/*数组c[0,n-1]和w[0,n-1]存放了n个字符及其概率，构造哈夫曼树HT*/
	int i, s1, s2;
	if (n <= 1) return;
	for (i = 1; i <= n; i++)
	{	/*根据n个权值构造n棵只有根结点的二叉树*/
		HT[i].ch = c[i - 1];
		HT[i].weight = w[i - 1];
		HT[i].parent = HT[i].lchild = HT[i].rchild = 0;
	}
	for (; i < 2 * n; ++i)
	{	/*初始化*/
		HT[i].parent = 0;
		HT[i].lchild = 0;
		HT[i].rchild = 0;
	}
	for (i = n + 1; i < 2 * n; i++)
	{	/*从HT[1..i-1]中选择parent为0且weight最小的两棵树，其序号为s1和s2*/
		select(HT, i - 1, s1, s2);	/*wy：select应为UDF*/
		HT[s1].parent = i;
		HT[s2].parent = i;
		HT[i].lchild = s1;
		HT[i].rchild = s2;
		HT[i].weight = HT[s1].weight + HT[s2].weight;
	}
}/*creatHTree*/

/*=====利用哈夫曼树生成前缀码=====*/
void HuffmanCoding(HuffmanTree HT, HuffmanCode HC, int n)
{
	char* cd;
	int i, start, c, f;
	if (n <= 1) return;
	cd = (char*)malloc(n * sizeof(char));
	cd[n - 1] = '\0';
	for (i = 1; i <= n; i++)
	{
		start = n - 1;
		for (c = i, f = HT[i].parent; f != 0; c = f, f = HT[f].parent)
		{
			if (HT[f].lchild == c) cd[--start] = '0';
			else cd[--start] = '1';
		}
		HC[i] = (char*)malloc((n - start) * sizeof(char));
		strcpy(HC[i], &cd[start]);
	}
	free(cd);
}/*HuffmanCoding*/

/*=====利用哈夫曼树译码=====*/
void Decoding(HuffmanTree HT, int n, char* buff)
{	/*n个叶子的HT译码，buff指向二进制序列*/
	int p = 2 * n - 1;
	while (*buff)
	{
		if ((*buff) == '0') p = HT[p].lchild;	/*进入左分支*/
		else p = HT[p].rchild;					/*进入右分支*/
		if (HT[p].lchild == 0 && HT[p].rchild == 0)
		{	/*到达一个叶子结点*/
			printf("%c", HT[p].ch);
			p = 2 * n - 1;						/*回到树根*/
		}/*if*/
		buff++;
	}/*while*/
}/*Decoding*/


 /*== 3.4.1 图 示例代码 ==============================================*/
 /*===================================================================*/
// 邻接矩阵表示的图的对应数据类型可定义为：
#define MaxN 30			// 图中顶点数的最大值
typedef int AdjMatrix[MaxN][MaxN];
// 或者
typedef double AdjMatrix[MaxN][MaxN];
typedef struct
{
	int Vnum;			// 图中的顶点数
	AdjMatrix Arcs;
}Graph;

// 邻接链表表示的图的对应数据类型可定义为：
#define MaxN 30			// 图中顶点数的最大值
typedef struct ArcNode			// 邻接链表的表结点
{
	int adjvex;					// 邻接顶点的顶点序号
	double weight;				// 边（弧）上的权值
	struct ArcNode *nextarc;	// 指向下一个邻接顶点的指针
}EdgeNode;
typedef struct VNode			// 邻接链表的头结点
{
	char data;					// 顶点表示的数据，以一个字符表示
	struct ArcNode *firstarc;	// 指向第一条依附于该结点的边（弧）的指针
}AdjList[MaxN];
typedef struct
{
	int Vnum;					// 图中顶点的数目
	AdjList Vertices;
}Graph;


/*== 3.4.2 图的遍历 示例代码 ========================================*/
/*===================================================================*/
// 深度优先搜索
int visited[MaxN] = { 0 };		// 调用访问前设置所有的顶点都没有被访问过
void Dfs(Graph G, int i) {
	EdgeNode *t; int j;
	printf("%d", i);			// 访问序号为i的顶点
	visited[i] = 1;				// 序号为i的顶点已经被访问过
	t = G.Vertices[i].firstarc;	// 取顶点的第一个邻接点
	while (t != NULL) {			// 检查所有与顶点i相邻接的顶点
		j = t->adjvex;			// 顶点j为顶点i的一个邻接点
		if (visited[j] == 0)	// 若顶点j未被访问则从j触发进行深度优先搜索
			Dfs(G, j);
		t = t->nextarc;			// 取顶点i的下一个邻接顶点
	}/*while*/
}/*Dfs*/

// 广度优先搜索
void Bfs(Graph G)
{
	EdgeNode *t; int i, j, k;
	int visited[MaxN] = { 0 };	// 遍历前设置所有顶点都未被访问
	InitQueue(Q);				// 创建一个空队列

}
