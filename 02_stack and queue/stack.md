### 栈(stack)：
栈是限定仅在**表尾进行插入或删除操作的线性表**，表尾端为**栈顶(top)**，表头端为**栈底(bottom)**。不含元素的空表称为**空栈**。栈的修改是按照**先进后出(Last in First Out, LIFO)** 的原则，其基本操作有**进栈/压栈push(插入元素)** 和 **出栈Pop(删除最后插入的元素)**。（参考弹夹）

**栈-存储结构+初始化**
```c
#define MAXSIZE = 100
typedef int ElemType;

typedef struct{
    ElemType data[MAXSIZE];
    // 栈顶指针就是top，其实不是一个指针，是栈顶的下标值
    int top;
}Stack;

void initStack(Stack *s)
{
    // 第一次压栈加1为0
    s->top = -1;
}
```
**栈-空栈判断**
```c
int isEmpty(Stack *s)
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
int push(Stack *s)
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
