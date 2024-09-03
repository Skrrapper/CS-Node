数据结构—-链表详解

**目录**

[TOC]



## 链表的定义

前面我们介绍的**顺序表**，在逻辑结构和物理结构上都是线性、连续的关系，那么我们在篇尾的时候也提到了是否有一种顺序表，无需在物理结构上连续，从而达到更好存取的目的呢？今天它来了。

**链表（LinkList）**属于**线性表**的一种，以下是百度百科关于链表的定义：

![image-20240508204723265](E:\note\image-20240508204723265.png)

总结下来，我们可以看出：

**在结构上**，**链表**并不是像顺序表那样底层结构是数组，而是包含两个区域：**数据域**、**指针域**。我们可以类比成火车，火车每一节车厢实际上是独立的，也就好比**数据域**，存储着各自的数据；但每一节车厢之间都会由一条链连接在一起，从而形成整个火车，链也就好比**指针域**，起到连接该车厢和指向下一车厢的作用。而实际上这样的存储结构也就是为什么**链表的物理结构可以不连续，而逻辑结构依旧是连续的**。

这样的好处就是当我们需要对指定元素进行移动、替换、存取等操作的时候，我们可以一步到位，无需挪动数据，无需扩容，不会造成空间上的浪费——好比你火车更换车厢，总不可能让整个火车都为你而改动吧。

![image-20240508210349403](E:\note\image-20240508210349403.png)

所以，如果要用一句话概括链表是什么，我们可以说：链**表是一种线性数据结构，由结点组成，每个结点包含一个数据元素也就是数据域和指向下一个节点的指针也就是指针域。**（叫法：结点或者节点都行）

![image-20240508210432210](E:\note\image-20240508210432210.png)

**联想：指针域的作用**

实际上，在后续的数据结构中，还有很多类型的数据结构会使用到指针域这个概念，或者可以说是结点的概念。结点指地是组成数据元素的存储映像，往往指针就是指向结点的，鉴于它不受物理空间限制的优点，往往都会使用指针和结点来更高效率地实现数据结构。

## 链表的构成

**数据域（data）：存放实际数据**

**指针域（next）：存放下一节点的首地址**

**结点：由数据域和指针域两部分信息组成的数据元素的存储映像**

## 链表的分类

我们平常使用的最多的就是**单链表**——**不带头单向不循环链表**

既然有**不带头**，那么必然就有**带头**；既然有**单向**，那么必然就有**双向**；既然有**不循环**，那么必然就有**循环**。

接下来针对这三个方向进行介绍。

### 双向和单向

**单向链表**：每个节点包含一个指向下一个节点的指针。单向链表只能从头节点开始遍历，无法从尾节点向前遍历。
**双向链表**：每个节点包含一个指向下一个节点和一个指向前一个节点的指针。双向链表可以从头节点或尾节点开始遍历，可以方便地在链表中间插入或删除节点。（向的描述与指针域的描述是一致的，单向只有一个指针域，而双向有两个）

可以通俗地理解为，**单向为单行道，只能从头走到尾；双向为双行道，既可退又可进。**

### 带头和不带头

**带头链表**：指第一个结点叫做头结点，不存储有效数据，只作为指引结点
**不带头链表**：指第一个结点不叫做头结点，就是第一个结点，存储有效数据

注意**这里指的带头就是带有头结点的意思**，头结点不存储任何数据，它仅仅用于指向第一个结点。优点是操作链表时统一处理，不需要特殊处理第一个节点，代码更加简洁清晰。缺点是需要额外的头节点，占用额外的空间。

### 循环和不循环

**循环链表**：最后一个节点的指针指向第一个节点，形成一个环状结构。循环链表可以从任意节点开始遍历，但需要注意避免出现死循环的情况。
**不循环链表**：最后一个节点的指针直接指向空，仅仅作为线性结构。

虽然链表种类之多，例如还有带有尾结点的链表等等，但在实际应用中只有两种：**单链表（不带头单向不循环链表）**和**双向链表（带头双向循环链表）**使用得最多，因为它们两个的特性加起来即为所有特性，而其他类型的链表也就不难创建了。

## 链表的命名

```c
typedef int SLTDataType;
typedef struct SListNode
{
     SLTDataType data;
     struct SListNode* next; 
}SLTNode
```

单链表

```c
SLTDataType、ElemType、LinklistType等等（根据自己的偏好设定），以下都命名为SLTDataType
```

数据域

```c
SLTDataType data; //保存结点数据
```

指针域（*

```c
struct SListNode* next;//保存下一结点地址
```

操作

```c
SLTPushBack//针对操作的命名一般在操作前缀添加SLT等代表其为链表的大写字母缩写
```



## 基本操作的实现

有关链表的操作，下方是一些举例

```c

//链表的头插、尾插
void SLTPushBack(SLTNode** pphead, SLTDataType x);
void SLTPushFront(SLTNode** pphead, SLTDataType x);

//链表的头删、尾删
void SLTPopBack(SLTNode** pphead);
void SLTPopFront(SLTNode** pphead);

//查找
SLTNode* SLTFind(SLTNode** pphead, SLTDataType x);

//在指定位置之前插入数据
void SLTInsert(SLTNode** pphead, SLTNode* pos, SLTDataType x);
//在指定位置之后插入数据
void SLTInsertAfter(SLTNode* pos, SLTDataType x);

//删除pos节点
void SLTErase(SLTNode** pphead, SLTNode* pos);
//删除pos之后的节点
void SLTEraseAfter(SLTNode* pos);

//销毁链表
void SListDesTroy(SLTNode** pphead);
//打印
void SLTPrint(SLTNode* phead);
```

接下来针对上述操作进行详细介绍。

### 初始化

即构造一个空链表

```c
void SlistTest01() {
	//一般不会这样去创建链表，这里只是为了给大家展示链表的打印
	SLTNode* node1 = (SLTNode*)malloc(sizeof(SLTNode));
	node1->data = 1;
	SLTNode* node2 = (SLTNode*)malloc(sizeof(SLTNode));
	node2->data = 2;
	SLTNode* node3 = (SLTNode*)malloc(sizeof(SLTNode));
	node3->data = 3;
	SLTNode* node4 = (SLTNode*)malloc(sizeof(SLTNode));
	node4->data = 4;
//当我们定义好了数据域之后，指针域应该如何定义呢？
    node1->next = node2;
	node2->next = node3;
	node3->next = node4;
	node4->next = NULL;
//像这样，让指针域指向该节点的下一节点，达到链接的目的
```

```c
SLTNode* SLTBuyNode(SLTDataType x) {
	SLTNode* newnode = (SLTNode*)malloc(sizeof(SLTNode));
	if (newnode == NULL) {//如果开辟失败的情况
		perror("malloc fail!");
		exit(1);
	}
	newnode->data = x;//开辟数据域
	newnode->next = NULL;//开辟指针域

	return newnode;
}
```



### 打印

在接下来承担显示的作用，将链表的结构可视化

```c
void SLTPrint(SLTNode* phead) {
	SLTNode* pcur = phead;
	while (pcur)
	{
		printf("%d->", pcur->data);
		pcur = pcur->next;//使用next指针指向下一节点
	}
	printf("NULL\n");
}
```



### 取值

与顺序表的取值不一样，因为链表物理结构是不连续的，所以在链表中进行访问的时候不能直接随机访问，只能从首元素出发遍历进行访问。

```c
Status SLTGet(LinkList L, int i, ElemType& e)//获取线性表L中的耨个数据元素的内容，通过变量e返回
{ 
	p = L->next;                    //初始化，p指向首元结点，
	j = 1;                          //初始化，j为计数器
	while (p&&j < i)                //向后扫描，直到p指向的第i个元素或p为空
	{
		p = p->next;                //p指向下一个结点
		++j;
	}
	if (!p || j > i)                //第i个元素不存在，抛出异常
		return NULL;   
	e = p->data;                    //取第i个元素
	return OK;
}

```



### 查找

查找操作同顺序表类似，都是哦才能够首元结点开始，依次将元素与给定值进行比较。

```c
//查找
SLTNode* SLTFind(SLTNode** pphead, SLTDataType x) {
	assert(pphead);

	//遍历链表
	SLTNode* pcur = *pphead;
	while (pcur) //等价于pcur != NULL
	{
		if (pcur->data == x) {
			return pcur;
		}
		pcur = pcur->next;
	}
	//没有找到
	return NULL;
}
```



### 插入

插入分为**尾插**、**头插**、**指定位置插入**，而指定位置插入又分为**指定位置之前和之后**。

**尾插**

```c
//尾插
void SLTPushBack(SLTNode** pphead/*为什么是**二级指针？因为*pphead这个指针在传参时实际上还是传值而不是传地址，需要传这个指针的地址也就是使用二级指针*/, SLTDataType x) {
	assert(pphead);

	SLTNode* newnode = SLTBuyNode(x);

	//链表为空，新节点作为phead
	if (*pphead == NULL) {
		*pphead = newnode;
		return;
	}
	//链表不为空，找尾节点
	SLTNode* ptail = *pphead;
	while (ptail->next)//并非ptial不能为空，而是ptail->next不能指向空
	{
		ptail = ptail->next;
	}
	//ptail就是尾节点
	ptail->next = newnode;
}
```

**头插**

```c
//头插
void SLTPushFront(SLTNode** pphead, SLTDataType x) {
	assert(pphead);
	SLTNode* newnode = SLTBuyNode(x);

	//newnode *pphead
	newnode->next = *pphead;
	*pphead = newnode;
}
```

### 指定位置插入

**之前**

```c
//在指定位置之前插入数据
//关键：找到前驱节点
void SLTInsert(SLTNode** pphead, SLTNode* pos, SLTDataType x) {
	assert(pphead);
	assert(pos);
	//要加上链表不能为空，如果链表都空了，必然找不到前驱节点
	assert(*pphead);

	SLTNode* newnode = SLTBuyNode(x);
	//pos刚好是头结点
	if (pos == *pphead) {
		//头插
		SLTPushFront(pphead, x);
		return;
	}

	//pos不是头结点的情况（如果不分情况讨论，那么prev就永远找不到pos）
	SLTNode* prev = *pphead;
	while (prev->next != pos)
	{
		prev = prev->next;
	}
	//prev -> newnode -> pos
	prev->next = newnode;
	newnode->next = pos;
}
```

**之后**

```c
//在指定位置之后插入数据
void SLTInsertAfter(SLTNode* pos, SLTDataType x) {
	assert(pos);

	SLTNode* newnode = SLTBuyNode(x);
    //错误操作：（顺序错误，导致pos->next不再指向下一节点
	//pos->next=newnode 
    //newnode->next=pos->next
	
    //正确操作：
    newnode->next = pos->next;
	pos->next = newnode;
}
```



### 删除

删除又分为**尾删**、**头删**、**指定结点删除以及指定位置后续结点删除**

**尾删**

```c
//尾删
void SLTPopBack(SLTNode** pphead) {
	assert(pphead);//第一个节点不能为空
	//链表不能为空
	assert(*pphead);

	//链表不为空

	if ((*pphead)->next == NULL)//如果只有一个节点 
    {
		free(*pphead);//直接释放
		*pphead = NULL;//置空
		return;
	}
    //如果有多个节点
	SLTNode* ptail = *pphead;
	SLTNode* prev = NULL;
	while (ptail->next)//不能指向空
	{
		prev = ptail;
		ptail = ptail->next;
	}
	
	prev->next = NULL;
	//销毁尾结点
	free(ptail);
	ptail = NULL;
}
```

**头删**

```c
//头删
void SLTPopFront(SLTNode** pphead) {
	assert(pphead);
	//链表不能为空
	assert(*pphead);

	//让第二个节点成为新的头
	//把旧的头结点释放掉
	SLTNode* next = (*pphead)->next;
	free(*pphead);
	*pphead = next;
}
```

### 删除

删除又分为**删除指定结点**、**删除指定结点之后的所有结点**

**删除pos结点**

```c
//删除pos节点
void SLTErase(SLTNode** pphead, SLTNode* pos) {
	assert(pphead);
	assert(*pphead);
	assert(pos);

	//pos刚好是头结点，没有前驱节点，执行头删
	if (*pphead == pos) {
		//头删
		SLTPopFront(pphead);
		return;
	}

	SLTNode* prev = *pphead;
	while (prev->next != pos)
	{
		prev = prev->next;
	}
	//找到了prev以及pos pos->next
	//先改变指向
    prev->next = pos->next;
	//再释放节点
    free(pos);
	pos = NULL;
}
```

**删除pos之后的结点**

```c
//删除pos之后的节点，也就是pos->next->next
void SLTEraseAfter(SLTNode* pos) {
	assert(pos);
	//pos->next不能为空
	assert(pos->next);

	//pos  pos->next  pos->next->next
	SLTNode* del = pos->next;//
	pos->next = pos->next->next;
	free(del);
	del = NULL;
}
```



### 销毁

```c
//销毁链表
//注意链表的空间是不连续的，所以不能一次性销毁所有元素，只能使用循环一个元素一个元素销毁
void SListDesTroy(SLTNode** pphead) {
	assert(pphead);
	assert(*pphead);

	SLTNode* pcur = *pphead;
	while (pcur)
	{
		SLTNode* next = pcur->next;
		free(pcur);
		pcur = next;
	}
	*pphead = NULL;
}
```

…

以上操作都基于单链表

## 部分其他链表的代码实现

### 循环链表

在介绍链表的分类的时候，已经介绍了循环链表的基本定义，也就是链表的最后一个结点的指针域指向第一个结点，这样就形成了一个循环链表，如果用图像来表示关系的话，如下：
![image-20240512194705924](E:\note\image-20240512194705924.png)

那么针对其代码的实现，实际上只需要让最后一个结点的指针域不指向空，而是指向第一个结点即可。

```c
//关键代码
p=B->next->next;
B->next=A->next;
A->next=p;//指向头结点
```

**循环链表的作用以及使用场景**

1. 约瑟夫问题：约瑟夫问题是一个经典的问题，即有n个人围成一圈，从第一个人开始报数，报到m的人出列，然后从出列的下一个人开始重新报数，直到所有人都出列。循环链表可以很好地模拟这个问题。
2. 环形队列：循环链表可以用来实现环形队列，即队列的尾节点指向头节点，可以很方便地实现循环入队和出队操作。
3. 循环播放列表：循环链表可以用来实现循环播放音乐列表或视频列表等功能。实际上循环这个概念，在生活中许多部分都有被使用到，而当它需要使用代码实现的时候，那么循环链表是较为容易实现的方案。

### 双向链表

双向链表的每个节点都包含两个指针，一个指向前一个节点，一个指向后一个节点。鉴于这个特点，它与单向链表不同的是，双向链表可以从头到尾或从尾到头遍历链表。

在双向链表中，通常有一个头节点和一个尾节点，它们分别指向链表的第一个节点和最后一个节点，可以方便地在头部或尾部进行插入或删除操作。

双向链表的操作普遍上比单向链表简单，因为它多了一个指针域所以操作的灵活性大大提高。

**双向链表的作用以及使用场景**

1. 需要频繁在链表中间插入或删除节点的情况：双向链表可以在O(1)的时间复杂度内完成插入或删除操作，因此适合在需要频繁插入或删除节点的场景中使用。
2. 需要双向遍历链表的情况：双向链表可以方便地从头到尾或从尾到头遍历链表，因此适合在需要双向遍历链表的场景中使用。
3. 需要实现栈或队列的情况：双向链表可以方便地在两端进行插入或删除操作，因此适合用来实现栈或队列。
4. 需要实现LRU缓存淘汰算法的情况：LRU缓存淘汰算法中经常需要删除最近最少使用的节点，双向链表可以方便地删除尾节点，因此适合用来实现LRU缓存淘汰算法。



## 优点/缺点（对比顺序表）

### 优点

**1.存储空间充足，对内存利用率高**

链表无需像顺序表那样预先分配空间，只要内存空间允许，链表中的元素个数就没有限制，那么这也可以反向说明链表对内存的利用率较高。

**2.插入和删除的效率高**

不像顺序表那样，在进行插入和删除的时候需要移动整个表，链表可以直接对单个元素进行插入和删除操作。

**3.动态性和灵活性**

链表的大小可以动态地调整，可以根据需要动态地插入或删除元素，不需要提前指定大小。

**4.可以实现高级数据结构**

实际上，在后续更高阶的数据结构中，许多结构都是基于链表的。链表可以实现栈、队列、哈希表等高级数据结构，具有很高的灵活性和扩展性。

### 缺点

**1.存储密度小，单个结点有效数据占用空间小**

我们发现，链表中的一个结点包含数据域和指针域，但是实际上真正存储了有效元素的只有数据域一部分，那么这就说明了其存储密度小（存储密度=数据元素本身占用的存储量/结点结构占用的存储量）

**2.存取元素的效率低**

链表不像顺序表那样，是随机存取结构，可以随时存取该位置上的元素；它属于顺序存取结构，只能通过遍历来实现存取元素操作。

**3.每存一个数据都要开辟动态空间，增加了内存分配的开销，并可能导致内存碎片化。所以说，动态化既有利也有弊。**

