### 单链表  

#### 1.双指针（快慢指针）
存在两个位置间隔为n的指针，分别为快指针fast和慢指针slow，n的大小由具体问题而定，在快指针和慢指针的同步循环过程中解决问题。

**查找链表倒数第k个位置的节点**
```c
// 使用双指针找到倒数第k个节点
int findNodeFS(Node* L, int k)
{
    Node* fast = L->next;
    Node* slow = L->next;
    for (int i = 0; i < k; i++)
    {
        fast = fast->next;
    }
    while(fast != NULL)
    {
        fast = fast->next;
        slow = slow->next;
    }
    printf("倒数第%d个节点的值为：%d\n", k, slow->data);
}
```
**删除链表中间节点**
```c
int delMiddleNode(Node* head)
{
    Node* fast = head->next;
    Node* slow = head;
    while(fast != NULL && fast->next != NULL)
    {
        fast = fast->next->next;
        slow = slow->next;
    }
    // slow 是中间节点的前一个节点
    Node* q = slow->next;
    slow->next = q->next;
    free(q);
    return 1;
}
```

**查找相同后缀开始位置的节点**
![](.\image\02_image01.PNG)
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

Node* findIntersectionNode(Node* headA, Node* headB)
{
    if(headA == NULL || headB == NULL)
    {
        return NULL;
    }
    Node* p = headA;
    int lenA = 0, lenB = 0, step;
    lenA = listLength(headA);
    lenB = listLength(headB);

    Node* fast;
    Node* slow;
    if (lenA > lenB)
    {
        step = lenA - lenB;
        fast = headA;
        slow = headB;
    }
    else
    {
        step = lenB - lenA;
        fast = headB;
        slow = headA;
    }
    
    //fast 先走step步
    for (int i = 0; i < step; i++)
    {
        fast = fast->next;
    }

    // 双指针同步走直到指向同一个节点退出循环
    while(fast != slow)
    {
        fast = fast->next;
        slow = slow->next;
    }
    return fast;
}
```
#### 2.拿空间换时间
![](.\image\02_image02.PNG)
**建立一个数组当下标使用，统计哪些数字出现过**
```c
void removeNode(Node* L int n)
{
    Node* p = L;
    int index;
    int* q = (int*)malloc(sizeof(int) * (n+1));

    // 初始化数组
    for(int i = 0; i < n+1; i++)
    {
        *(q + i) = 0;
    }
    while(p->next != NULL)
    {
        index = abs(p->next->data);
        if(*(q + index) == 0)
        {
            *(q + index) = 1;
            p = p->next;
        }
        else
        {
            Node* temp = p->next;
            p->next = temp->next;
            free(temp);
        }
    }
    free(q);
}
```
#### 3.反转链表
**将链表进行反转**
```c
Node* reverseList(Node* head)
{
    Node* first = NULL;
    Node* second = head->next;
    Node* third;

    while(second != NULL)
    {
        third = second->next;
        second->next = first;
        first = second;
        second = third;
    }
    Node* hd = initList();
    hd->next = first;
    return hd;
}
```
#### 小练习
![](.\image\02_image03.PNG)
**解法：找到中间节点进行断开，然后交叉指向**
```c
void reOrderList(Node* head)
{
    Node* fast = head->next;
    Node* slow = head;
    while(fast != NULL && fast->next != NULL)
    {
        fast = fast->next->next;
        slow = slow->next;
    }

    Node* first = NULL;
    Node* second = slow->next;
    slow->next = NULL;
    Node* third = NULL;

    while(second != NULL)
    {
        third = second->next;
        second->next = first;
        first = second;
        second = third;
    }

    Node* p1 = head->next;
    Node* q1 = first;
    Node* p2, * q2;
    while(p1 != NULL && q1 != NULL)
    {
        p2 = p1->next;
        q2 = q1->next;

        p1->next = p1;
        q1->next = p2;

        p1 = p2;
        q1 = q2;
    } 
}
```
**单链表的局限性是只能单向指向，所以引出单向循环链表和循环链表**

### 循环单链表
循环链表（Circular Linked List）是另一种形式的链式存储结构，其特点是最后一个节点指向头节点，整个链表形成一个环。当链表遍历是循环单链表 的判别条件为 p != L 或 p->next != L。
![](.\image\02_image04.PNG)
#### 1.判断环
**利用快慢指针，快指针比慢指针走的快，如果能相遇，则有环**
```c
int isCycle(Node* head)
{
    Node* fast = head;
    Node* slow = head;
    
    while(fast != NULL && fast-> != NULL)
    {
        fast = fast->next->next;
        slow = slow->next;
        if(fast == slow)
        {
            return 1;
        }
    }
    return 0;
}
```

#### 2.找到链表环的入口
**利用快慢指针先确定环的大小，再遍历出环的入口**
```c
Node* findBegin(Node* head)
{
    Node* fast = head;
    Node* slow = head;
    while(fast != NULL && fast-> != NULL)
    {
        fast = fast->next->next;
        slow = slow->next;
        if(fast == slow)
        {
            Node* p = fast;
            int count = 1;
            while(p->next != slow)
            {
                count++;
                p = p->next;
            }
            fast = head;
            slow = head;

            for(int i = 0; i < count; i++)
            {
                fast = fast->next;
            }
            while(fast != slow)
            {
                fast = fast->next;
                slow = slow->next;
            }
            return slow;
        }
    }
    return NULL;
}
```
### 双向链表
**单链表中，找直接后继的执行时间为O(1)，找直接前驱的执行时间为O(n)---->双向链表（Double Linked List），其节点中有两个指针域，，一个指向直接后继，另一个指向直接前驱。**  

**双向链表-存储结构**
```c
typedef int ElemType;

typedef struct node{
    ElemType data;
    struct node* prev,* next;
}Node;
```
**双向链表-头插法**
```c
int insertHead(Node* L, ElemType e)
{
    Node* p = (Node*)malloc(sizeof(Node));
    p->data = e;
    p->prev = L;
    p->next = L->next;
    if(L->next != NULL)
    {
        L->next->prev = p;
    }
    L->next = p;
    return 1;
}
```
**双向链表-遍历同单链表相同**

**双向链表-尾插法**
```c
Node* insertTail(Node* tail, ElemType e)
{
    Node* p = (Node*)malloc(sizeof(Node));
    p->data = e;
    p->prev = tail;
    tail->next = p;
    p->next = NULL;
    return p;
}
```
**双向链表-指定位置插入数据**
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
    q->prev = p;
    q->next = p->next;
    p->next->prev = q;
    p->next = q;
    return 1;
}
```
**双向链表-删除节点**  
```c
int deleteNode(Node* L, int pos)
{
    // 删除节点的前驱
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
    q->next->prev = p;
    free(q);
    return 1;
}
```
**双向链表-释放链表同单链表相同**