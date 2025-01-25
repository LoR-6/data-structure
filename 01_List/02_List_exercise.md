### 单链表应用
#### 1.双指针（快慢指针）
存在两个位置间隔为n的指针，分别为快指针fast和慢指针slow，n的大小由具体问题而定，在快指针和慢指针的同步循环过程中解决问题。

**查找链表倒数第k个位置的节点**
```c
// 使用双指针找到倒数第k个节点
int findNodeFS(Node* L int k)
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

**查找相同后缀开始位置的节点**
![]()

```c
// 使用双指针找到倒数第k个节点
int findNodeFS(Node* L int k)
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