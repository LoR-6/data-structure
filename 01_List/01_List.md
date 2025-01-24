### 线性表：
由n (n >= 0) 个数据特性相同的元素构成的有限序列，其存储结构包括两种：顺序存储结构和链式存储结构。 

#### 一、顺序表

顺序表：即线性表的顺序存储结构，用一组连续的内存单元依次存储线性表的各个元素，也就是说，逻辑上相邻的元素实际上的物理存储空间也是连续的  

**顺序表-存储结构**
```c
#define MAXSIZE = 100
typedef int ElemType;

typedef struct{
    ElemType data[MAXSIZE];
    int length;
}SeqList;
```
**顺序表-初始化**
```c
#define MAXSIZE = 100
typedef int ElemType;

typedef struct{
    ElemType data[MAXSIZE];
    int length;
}SeqList;

void initList(SeqList* L)
{
    L->length = 0;
}
```  
**顺序表-在尾部添加元素**
```c
int appendElem(SeqList* L, ElemType e)
{
    if(L->length >= MAXSIZE)
    {
        printf("顺序表已满\n");
        return 0;
    }
    L->data[L->length] = e;
    L->length++;

    return 1;
}
```
**顺序表-遍历**
```c
int listElem(SeqList* L)
{
    for (int i = 0; i < L->length; i++)
    {
        printf("%d", L->data[i]);
    }
    printf("\n");
}
```
**顺序表-插入元素**  
（对应位置插入元素，插入位置的后继元素从后往前均往后移动一位）  
先移动后插入
```c
int insertElem(SeqList* L, int pos, ElemType e)
{
    // 省略了判断，出现错误return 0
    // pos的位置是从1开始
    if (pos <= L->length)
    {
        for (int i = L->length-1; i >= pos-1; i--)
        {
            L->data[i+1] = L->data[i];
        }
        L->data[pos-1] = e;
        L->length++;
    }
    return 1;
}
```
顺序表插入数据的最好时间复杂度为O(1):在最后一个位置插入  
顺序表插入数据的最坏时间复杂度为O(n):在第一个位置插入
  
**顺序表-删除元素**  
删除位置的后继元素全部往前移动，最后一个不用管，直接修改长度
```c
int deleteElem(SeqList* L, int pos, ElemType* e)
{
    // 省略了判断，出现错误return 0
    // pos的位置是从1开始
    // e 返回为被删除的值
    *e = L->data[pos - 1];
    // 如果删除最后一个元素，直接修改长度即可
    if (pos < L->length)
    {
        for (int i = pos; i < L->length; i++)
        {
            L->data[i-1] = L->data[i];
        }  
    }
    L->length--;
    return 1;
}
```

**顺序表-查找元素**  
删除位置的后继元素全部往前移动，最后一个不用管，直接修改长度
```c
int findElem(SeqList* L, ElemType e)
{
    // 省略了判断，出现错误return 0
    
    for (int i = 0; i < L->length; i++)
    {
        if(L->data[i] == e)
        {
            return i + 1;
        }
    }
    return 0;
}
```

**顺序表-动态分配内存地址初始化**  
```c
typedef struct{
    Elemtype *data;
    int length;
}SeqList;

SeqList* initList()
{
    SeqList* L = (SeqList*)malloc(sizeof(SeqList));
    L->data = (ElemType*)malloc(sizeof(ElemType) * MAXSIZE);
    L->length = 0;
    return L;
}
```
除此之外还有一些简单的函数，比如返回顺序表长度 = 返回 length，清空顺序表 = length置0
  
#### 二、链表
顺序表在数据的插入和删除等操作，需要移动数据表中的其他数据，这一不便之处也促使了另一种线性表的出现——链表。  
链表为了表示每个元素和其后继元素的逻辑关系，每个数据元素除了其本身的信息（存储数据元素信息的数据域），还要存储一个指示其直接后继的信息（存储直接后继存储位置的指针域），两者共同构成节点（node）。  
**头节点不存储任何数据，如：int则为0，尾节点指向的下一个地址是NULL**

**链表-存储结构**  
```c
typedef int ElemType;

typedef struct node{
    ElemType data;
    struct node* next;
}Node;
```

**单链表-初始化**  
```c
// 创建链表（其实是头节点）
Node* initList()
{
    Node* head = (Node*)malloc(sizeof(Node));
    head->data = 0;
    head->next = NULL;
    return head;
}

int main()
{
    Node* List = initList();
    return 1;
}
```

**单链表-头（前）插法**  
注：头插法的顺序和排列的顺序是相反的
```c
int insertHead(Node* L, ElemType e)
{
    Node* p = (Node*)malloc(sizeof(Node));
    p->data = e;
    // 先将新节点的指针域填写
    p->next = L->next;
    // 再将头节点的指针域指向新节点
    L->next = p;
}
```

**单链表-遍历**  
```c
void listNode(Node* L)
{
    Node* p = L->next;
    while(p != NULL)
    {
        printf("%d\n",p->data);
        p = p->next;
    }
    printf("\n");
}
```

**单链表-尾（后）插法**  
```c
// 获取尾节点
int get_tail(Node* L)
{
    Node* p = L;
    while(p->next != NULL)
    {
        p = p->next;
    }
    return p;
}

// 插入数据
Node* insertTail(Node *tail, ElemType e)
{
    Node* p = (Node*)malloc(sizeof(Node));
    p->data = e;
    tail->next = p;
    p->next = NULL;
    return p;
}
```

**单链表-指定位置插入数据**  
```c
int insertNode(Node* L, int pos, ElemType e)
{
    // 用来保存插入位置的前驱节点
    Node* p = L;
    int i = 0;
    // 遍历链表找到插入位置的前驱节点
    while (i < pos - 1)
    {
        p = p->next;
        i++;
        if(p == NULL)
        {
            return 0;
        }
    }
    // 要插入的新节点
    Node* q = (Node*)malloc(sizeof(Node));
    q->data = e;
    q->next = p->next;
    p->next = q;
    return 1;
}
```

**单链表-删除节点**  
找到删除节点的前置节点p--->
用指针q记录要删除的节点--->
通过改变p的后继节点实现删除--->
释放删除节点的空间
```c
int deleteNode(Node* L, int pos)
{
    // 用删除节点的前驱
    Node* p = L;
    int i = 0;
    // 遍历链表找到插入位置的前驱节点
    while (i < pos - 1)
    {
        p = p->next;
        i++;
        // pos 超出链表的实际长度
        if(p == NULL)
        {
            return 0;
        }
    }
    // 要删除的节点不存在
    if (p->next == NULL)
    {
        printf("要删除的位置错误\n");
        return 0;
    }

    Node* q = p->next;
    p->next = q->next;
    free(q);
    return 1;
}
```

**单链表-获取链表的长度**  
```c
int listLength(Node* L)
{
    Node* p = L;
    int len = 0;
    while(p != NULL)
    {
        p = p->next;
        len++;
    }
    return len;
}
```

**单链表-释放链表**  
指针p指向头节点后的第一个节点--->
判断指针p是否指向空节点--->
如果p不为空，用指针q记录指针p的后继节点--->
释放指针p指向的节点--->
指针p和指针q指向同一个节点，循环上面操作。
```c
int freeList(Node* L)
{
    Node* p = L->next;
    Node* q;

    while(p != NULL)
    {
        q = p->next;
        free(p);
        p = q;
    }
    L->next = NULL;
}
```

**顺序表读取速度较快，链表修改数据的速度较快**
