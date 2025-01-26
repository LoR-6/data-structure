### 单链表应用  

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