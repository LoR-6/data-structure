### 栈(stack)：
栈是限定仅在**表尾进行插入或删除操作的线性表**，表尾端为**栈顶(top)**，表头端为**栈底(bottom)**。不含元素的空表称为**空栈**。栈的修改是按照**先进后出(Last in First Out, LIFO)** 的原则，其基本操作有**进栈/压栈push(插入元素)** 和 **出栈Pop(删除最后插入的元素)**。（参考弹夹）
#### 1.存储结构
**栈-存储结构+初始化**
```c
#define MAXSIZE = 100
typedef int ElemType;

typedef struct{
    ElemType data[MAXSIZE];
    // 栈顶指针就是top，其实不是一个指针，是栈顶的下标值
    int top;
}Stack;

void initStack(Stack* s)
{
    // 第一次压栈加1为0
    s->top = -1;
}
```
**栈-空栈判断**
```c
int isEmpty(Stack* s)
{
    if(s->top == -1)
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
**栈-进栈/压栈**
```c
int push(Stack* s)
{
    if(s->top >= MAXSIZE-1)
    {
        printf("满了\n")；
        return 0;
    }
    s->top++;
    s->data[s->top] = e;
    return 1;
}
```
**栈-出栈**
```c
int pop(Stack* s, ElemType* e)
{
    if(s->top == -1)
    {
        printf("空的\n")；
        return 0;
    }
    *e = s->data[s->top];
    s->top--;
    return 1;
}
```
**栈-动态分配内存地址**  
```c
#define MAXSIZE = 100
typedef int ElemType;

typedef struct{
    Elemtype *data;
    int top;
}Stack;

Stack* initStack()
{
    Stack* s = (Stack*)malloc(sizeof(Stack));
    s->data = (ElemType*)malloc(sizeof(ElemType) * MAXSIZE);
    s->top = -1;
    return s;
}
```
#### 2.链式结构
**栈-链式结构+初始化**
```c
typedef int ElemType;

typedef struct stack{
    ElemType data；
    struct stack* next;
}Stack;

Stack* initStack()
{
    Stack* s = (Stack*)mallloc(sizeof(Stack));
    s->data = 0;
    s->next = NULL;
    return s;
}
```
**栈-空栈判断**
```c
int isEmpty(Stack* s)
{
    if(s->next == NULL)
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
**栈-进栈（链式结构的栈顶是头节点后的节点，压栈就是头插法）**
```c
int push(Stack* s, ElemType e)
{
    Stack* p = (Stack*)malloc(sizeof(Stack));
    p->data = e;
    p->next = s->next;
    s->next = p;
    return 1;
}
```
**栈-出栈（出栈是删除头节点后的节点）**
```c
int pop(Stack* s, ElemType *e)
{
    if(s->next == NULL)
    {
        printf("空的\n");
        return 0;
    }
    *e = s->next->data;
    Stack* q = s->next;
    s->next = q->next;
    free(q);
    return 1;
}
```
**栈-获取栈顶元素**
```c
int getTop(Stack* s, ElemType* e)
{
    if(s->next == NULL)
    {
        printf("空的\n");
        return 0;
    }
    *e = s->next->data;
    return 1;
}
```