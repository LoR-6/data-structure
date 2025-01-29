### 队列(queue)：
队列**只**允许**表的一端插入，另一端删除元素**的线性表，**允许插入的一端**称为**队尾(rear)**，**允许删除的一端**为**队头(front)**。队列的修改是按照**先进先出(First in First Out, FIFO)** 的原则，其基本操作有**入队** 和 **出队**。（参考排队）

#### 一、队列存储结构
**队列-存储结构+初始化**
```c
#define MAXSIZE = 100
typedef int ElemType;

typedef struct{
    ElemType data[MAXSIZE];
    // 
    int front, rear;
}Queue;

void initQueue(Queue* Q)
{
    //可以为0或者-1
    Q->front = 0;
    Q->rear = 0;
}
```
**队列-判断队列是否为空**
```c
int isEmpty(Queue* Q)
{
    if(Q->front == Q->rear)
    {
        printf("空的\n")；
        return 1;
    }
    else
    {
        return 0;
    }
}
```
**队列-出队**
```c
ElemType dequeue(Queue* Q)
{
    if(Q->front == Q->rear)
    {
        printf("空的\n")；
        return 0;
    }
    ElemType e = Q->data[Q->front];
    Q->front++;
    return e;
}
```

**队列-入队**
```c
// 调整队列
int queueFull(Queue* Q)
{
    if(Q->front > 0)
    {
        int step = Q->front;
        for (int i = Q->front; i <= Q->rear; ++i)
        {
            Q->data[i-step] = Q->data[i];
        }
        Q->front = 0;
        Q->rear = Q->rear - step;
        return 1;
    }
    else
    {
        printf("队列全满\n");
        return 0;
    }
}
// 入队
int equeue(Queue* Q, ElemType e)
{
    if(Q->rear >= MAXSIZE)
    {
        if(!queueFull(Q))
        {
            return 0;
        }
    }
    Q->data[Q->rear] = e;
    Q->rear++;
    return 1;
}
```
**队列-获取队头数据**
```c
int getHead(Queue* Q, ElemType* e)
{
    if(Q->front == Q->rear)
    {
        printf("空的\n")；
        return 0;
    }
    *e = Q->data[Q->front];
    return 1;
}
```
**队列-动态内存分配**
```c
#define MAXSIZE = 100
typedef int ElemType;

typedef struct{
    Elemtype *data;
    int front, rear;
}Queue;

Queue* initQueue()
{
    Queue* q = (Queue*)malloc(sizeof(Queue));
    q->data = (ElemType*)malloc(sizeof(ElemType) * MAXSIZE);
    q->front = 0;
    q->rear = 0;
    return q;
}
```

**循环队列：入队过程因假溢出需要调整队列，复杂度较高，因此引出循环队列**  

**循环队列-入队**
```c
int equeue(Queue* Q, ElemType e)
{
    if((Q->rear + 1) % MAXSIZE == Q->front)
    {
        printf("满了\n");
        return 0;
    }
    Q->data[Q->rear] = e;
    Q->rear = (Q->rear + 1) % MAXSIZE;
    return 1;
}
```
**循环队列-出队**
```c
int dequeue(Queue* Q, ElemType e)
{
    if(Q->rear == Q->front)
    {
        printf("空的\n");
        return 0;
    }
    *e = Q->data[Q->front];
    Q->front = (Q->front + 1) % MAXSIZE;
    return 1;
}
```
#### 二、队列链式结构
**队列-链式结构**
```c
typedef struct QueueNode{
    ElemType data；
    struct QueueNode* next;
}QueueNode;

typedef struct {
    struct QueueNode* front;
    struct QueueNode* rear;
}Queue;
```
**队列-初始化**
```c
Queue* initQueue()
{
    Queue* q = (Queue*)malloc(sizeof(Queue));
    QueueNode* node = (QueueNode*)malloc(sizeof(QueueNode));
    node->data = 0;
    node->next = NULL;
    q->front = node;
    q->rear = node;
    return q;
}
```
**队列-判断空队列**
```c
int isEmpty(Queue* Q)
{
    if(Q->front == Q->rear)
    {
        printf("空的\n")；
        return 1;
    }
    else
    {
        return 0;
    }
}
```
**队列-入队**
```c
void equeue(Queue* q, ElemType e)
{
    QueueNode* node = (QueueNode*)malloc(sizeof(QueueNode));
    node->data = e;
    node->next = NULL;
    q->rear->next = node;
    q->rear = node;
} 
```
**队列-出队**
```c
int dequeue(Queue* q, ElemType *e)
{
    QueueNode* node = q->front->next;
    *e = node->data;
    q->front->next = node->next;
    if(q->rear == node)
    {
        q->rear = q->front;
    }
    free(node);
    return 1;
} 
```
**队列-获得队头元素**
```c
ElemType getFront(Queue* q)
{
    QueueNode* node = q->front->next;
    if (isEmpty(q))
    {
        printf("空的\n");
        return 0;
    }
    return q->front->next->data;
} 
```