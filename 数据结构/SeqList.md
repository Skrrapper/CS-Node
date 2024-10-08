## 顺序表的定义

**顺序表（SeqList）**属于**线性表**的同一种，它同样具有线性的存储结构，以下是百度百科关于顺序表的定义：

![image-20240422104342592](C:\Users\Skrrapper\AppData\Roaming\Typora\typora-user-images\image-20240422104342592.png)

总结下来，
**在结构上**，**顺序表**实际上的**底层结构**就是数组，而顺序表本身也就是**对一个数组的封装以及修饰**；
**在元素上**，**顺序表**实际上就是**元素之间逻辑关系和物理关系一致的一种线性表**，与其对应的是链表（后续会谈及）。

下面针对这两个方面具体解释什么是顺序表。

### **结构上**

顺序表的底层结构通常是通过**数组**来实现的。数组是一种连续存储数据元素的数据结构，可以通过**下标**来访问数组中的元素。在顺序表中，**数组的下标对应顺序表中元素的位置，通过数组的下标可以快速定位和访问顺序表中的元素。**

顺序表通过数组来实现的优点是可以快速随机访问元素，插入和删除操作相对简单。需要注意，数组的大小是固定的，是不能随便改变大小的。

而基于数组的大小限制，我们将顺序表也就分为了两种：静态顺序表和动态顺序表。

顾名思义，静态顺序表就是不能改变大小的顺序表，而动态则可以改变。

**静态顺序表**使用定长数组存储元素。

```c
#define N 10
typedef int SLDataType
typedef struct SeqList_Static
{
  SLDataType a[N]
  int size;
}SL;
//无法改变空间大小，给大或者给小都会造成存储上的缺陷
```

**动态顺序表**按照需求申请空间容量。

```c
#define INIT_CAPACITY 4
typedef int SLDataType;
typedef struct SeqList
{
 SLDataType* a;
 int size; // 有效数据个数
 int capacity; // 空间容量
}SL;
//可以根据需求改变空间容量
```

总的来说，由于动态顺序表的特点，在需要频繁改变数组元素的场景，其的使用频率会远远大于静态顺序表，而实际上，在任何时候动态顺序表都是相对于静态更好的选择，毕竟它除了相较于静态的优点，其他同静态并无差异。

顺序表乃至整个线性表都需要初定义的存储空间，然而在存储的过程中，动态的存储往往是比静态存储要利大于弊的，所以我们在今后的使用之中，也应该尽量使用动态。

### 元素上

我们先来介绍逻辑结构以及顺序结构的基本含义。

#### **逻辑结构**

理论上，抽象的一条线，在逻辑上具有连续的关系

#### **物理结构**

物理上，具象的一条线，在物理上具有连续的关系

而顺序表在这两种结构上的特点是：

逻辑结构：**线性**

物理结构：**线性**

所以我们可以说：**顺序表**实际上就是**元素之间逻辑关系和物理关系一致的一种线性表**。

而正是由于这样的特点，关于顺序表的各种操作都会基于此来进行操作，代码的编写也会由于此而有一定的规律。

## 线性表的命名

**SeqList**：线性表

作为**Sequential List**的缩写。

**SLDataType**：自定义的数组空间

以上两个只是命名的例子之二，针对线性表的命名，通常都是在前缀或者后缀加上**SeqList**或者更加简短例如**List**、**SL**的缩写，这些命名规则都基于其是有关线性表的。

而像常规性的**capacity**、**size**等等这种较为普遍的命名，根据其大致英文意思即可理解其代表着什么。
命名的意义主要在于：后续操作使用的类型不必再使用int等常见类型，而是可以使用自定义命名的类型。这样大大提高了编写效率。

实际上，在代码编写中的命名都遵循着**具有可读性，不繁琐不复杂**的原则，具体的命名规则可参考这篇文章：

## 基本操作的实现

有关线性表的操作，下方是一些举例：

```c
//初始化
void Init(SqList& L);
//取值
void Get(SqList L, int i, ElemType& e);
//查找
void Find(SqList L, int i,ElemType e);
//销毁
void Destroy(SqList& L);
//打印
void Print(SqList& L); 

//顺序表的头部/尾部插入
void PushBack(SqList& L, int i, ElemType e);
void PushFront(SqList& L, int i, ElemType e);

//顺序表的头部/尾部删除
void PopBack(SqList& L, int i;
void PopFront(SqList& L, int i);

//删除指定位置数据
void Insert(SqList& L, int pos, ElemType x);
void Erase(SqList& L, int pos);

...
```

针对上方的操作，接下来针对其中几个进行详细介绍。

### 初始化

#### 基本定义

初始化的作用是给定一个空间，构造一个空的顺序表。此时的顺序表是一个**空表**，等待着数据存放其中。

#### 代码跟写

```c
Status Init(SqList& L)
{
	L.elem = new ElemType[MAXSIZE];//分配MAXSIZE大小的空间
	if (!L.elem) exit(OVERFLOW);//分配失败
	L.length = 0;//空表
	return OK;
}
```

### 取值

#### 基本定义

根据指定的位置序号，获取顺序表中第i个元素的值

#### 代码跟写

```c
Status Get(SqList L, int i, ElemType& e)
{
	if (i<1 || i>L.length) return ERROR;//判断i是否合理

	e = L.elem[i - 1];//下标-1存储第i个元素

	return OK;

}
```

### 查找

#### 基本定义

根据指定的元素e，查找顺序表中第i个值与e相等的元素。查找成功则返回其位置序号，否则返回0.

#### 代码跟写

```c
Status Find(SqList L, int i,ElemType e)
{

	for (i = 0; i < L.length; i++)//遍历
	if (L.elem[i] == e) return i + 1;//查找成功
	return 0;
}
```

### 插入

#### 基本定义

在表的头部或者尾部插入一个新的数据元素e；在表的第i个位置插入一个新的数据元素e

**尾插**
**A**.当尾部后有足够的空间时
直接插入：arr[size]

**B**.当已经满空间时
1.删除不需要的数据，再插入数据

2.扩大空间再插入数据：
扩充原则
a.一次扩充一个（效率低下）

b.一次扩充固定n个（相当于给一个静态顺序表，直接给n个）

c.成倍数扩充（1.5、2倍）（推荐）介于a和b方法之间，针对a：如果每次增容的空间是原来的两倍，那么在数组需要扩容时，只需要进行一次内存分配操作，而不是多次分配，这样可以减少内存分配的次数，提高程序的性能；而针对b：增容时选择按照倍数的方式增加空间，而不是一次性增加固定数量的空间，是因为按照倍数增加空间可以更好地平衡内存利用率和性能。既不多也不少刚刚好

**头插**

第0个位置插入元素，则需要将后续的数据统一向后挪一位

**指定位置插入元素**

注意的是，当我们需要指定位置插入一个新元素之前，表长会变为原来的n加上1，而这也说明了除非是插入在表的首位置或者是尾部，我们需要移动指定位置的元素来腾出位置给新元素。

#### 代码跟写

```c
//扩容函数，以2倍扩充
void SLCheckCapacity(SL* ps) 
{
	if (ps->size == ps->capacity) {
		int newCapacity = ps->capacity == 0 ? 4 : 2 * ps->capacity;//默认空间是4，并且以2倍扩充
		SLDataType* tmp = (SLDataType*)realloc(ps->arr, newCapacity * sizeof(SLDataType));
	    //扩充失败
        if (tmp == NULL)
        {
			perror("realloc fail!");
			exit(1);
		}
		//扩容成功
		ps->arr = tmp;
		ps->capacity = newCapacity;
	}
}

//尾插
void SLPushBack(SL* ps, SLDataType x) 
{
	//首先判断是否为空表
    //断言判断
	//assert(ps != NULL);
	assert(ps);

	//if判断
	if (ps == NULL) 
    {
		return;
	}

	//判断是否需要扩容
	SLCheckCapacity(ps);

	//空间足够，直接插入
	ps->arr[ps->size++] = x;
	//ps->size++;
}
//头插
void SLPushFront(SL* ps, SLDataType x) 
{
	assert(ps);

	//判断是否需要扩容
	SLCheckCapacity(ps);

	//原先的数据往后挪动一位
	for (int i = ps->size; i > 0; i--) //i = 1
	{
		ps->arr[i] = ps->arr[i - 1]; //ps->arr[1] = ps->arr[0]
	}
	ps->arr[0] = x;//在表头插入新数据
	ps->size++;
}

//指定位置之前插入数据
void SLInsert(SL* ps, int pos, SLDataType x)
{
	assert(ps);
	assert(pos >= 0 && pos <= ps->size);
	
	SLCheckCapacity(ps);

	//pos及之后的数据往后挪动一位，pos空出来
	for (int i = ps->size; i > pos ;i--)
	{
		ps->arr[i] = ps->arr[i - 1]; //ps->arr[pos+1] = ps->arr[pos]
	}
	ps->arr[pos] = x;//插入新数据
	ps->size++;
}
```

### 删除

### 基本定义

在表的头部或者尾部删除一个或者多个数据元素；在表的第i个位置删除一个或多个数据元素

与插入相似的理解，但是删除是删除元素，表长会变为原来的n减上1（只考虑删除一个元素），而这也说明了除非是删除表的首位置或者是尾部，我们在删除元素之后需要将空出的位置补齐。

#### 代码跟写

```c
//尾删
void SLPopBack(SL* ps) 
{
	assert(ps);
	assert(ps->size);

	//顺序表不为空
	//ps->arr[ps->size - 1] = -1;
	ps->size--;
}

//头删
void SLPopFront(SL* ps) 
{
	assert(ps);
	assert(ps->size);

	//不为空执行挪动操作
	for (int i = 0; i < ps->size-1 ;i++)
	{
		ps->arr[i] = ps->arr[i + 1];
	}
	ps->size--;
}

//删除指定位置数据
void SLErase(SL* ps, int pos) 
{
	assert(ps);
	assert(pos >= 0 && pos < ps->size); 

	//pos以后的数据往前挪动一位
	for (int i = pos;i < ps->size-1;i++)
	{
		ps->arr[i] = ps->arr[i + 1];//ps->arr[i-2] = ps->arr[i-1];
	}
	ps->size--;
}
```

## 优点/缺点

### 优点

1. **随机访问效率高**：由于顺序表中的元素在内存中是连续存储的，可以通过下标直接访问任意位置的元素，时间复杂度为O(1)**（查找操作）**。
2. **适合元素较少或固定大小的情况**：顺序表的存储结构相对简单，适用于元素数量较少或者固定大小的情况。然而其实反过来看这也可以称作它的一个缺点。
3. **内存利用率高**：顺序表**不需要额外的指针**来维护元素之间的关系，内存利用率更高，也可以说是其**存储密度大**。

### 缺点

1. **插入和删除操作效率低**：在顺序表中插入或删除元素时，需要将插入或删除位置后的所有元素依次向后或向前移动，时间复杂度为O(n)**（插入操作）**。
2. **不易扩展**：顺序表的大小是固定的，当元素数量超过顺序表的容量时，需要重新分配更大的内存空间并将元素复制到新的空间中，操作较为复杂。
3. **浪费空间**：顺序表在插入和删除元素时可能会导致内存空间的浪费，因为需要预留一定的空间以容纳未来的插入元素**（插入操作）**。

针对元素较少的线性表，我们使用顺序表是足以解决问题的，但是当元素多起来，在我们进行插入或者删除等操作的时候需要移动的元素就会越多，所要消耗的时间也会越多。**那我们是否有一种方法可以不需要移动元素，直接达到操作目的呢？当然是有的。**

**链式存储结构**可以帮助我们很好地解决这个问题，它规避顺序表所需要遵循的顺序结构，而是使用指针定位到元素，这样元素之间的物理结构不再是线性，从而也能更加方便地进行存取。而关于它的详细介绍，将在下一节讲解。

