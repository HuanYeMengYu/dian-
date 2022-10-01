# ChenZhaoqing's weekly reports
## Week1
### 1)learning hours:
       sum: roughly 20 hours
### 2)learning progress；
        I learnt the usage of C and created the linux environment .
        Now,I'm training my proficiency of linux instruction usage.
### 3）implementation：
        I commanded the basic usage of C.
        I knew the process of writing,compiling and running a C program.
        I can use gcc and gdb in the terminal.
        I configured the programming environment by writing the ".vscode" file on vscode, realizing the "auto-compiling".
        I can use git to synchronize my personal code on Github.
        I used Centos7 on vmware previously, but now I use Ubuntu20.04 on vmware to creat linux environment.
        I commanded the usage of mobaxterm, and successfully conneted my Ubuntu on vmware,so that I can program on mobaxterm succinctly.
        If the team offers the server ,I think I can connet it successfully.
### 4)problems：
     the writing of config file、configuring public and private keys for password-free login
### 5)plan;
      First,I'll go deep into the usage of insutruction in Linux（Ubuntu20.04），guaranteeing that I can develop in Linux environment expertly and efficiently.
      Next, I'll learn more about SSH and the writing of "config" file.
      Maybe,I'll try to learn the basic usage of C++.
      Then,I'll learn the knowledge of computer network to implement the task "echo server"， and if time permitting,I'll learn the multithreading 
      programming.
### 6）some C code of mine：
#### 可变链表：
#include <stdio.h>
#include <stdlib.h>

struct node{     //定义链表的一个节点
    int value;
    struct node *next;
};

struct list{            //封装
struct node *head;     //相当于一个二级指针
};

void add(struct list* plist,int num);  //给链表增加节点的函数
void print(struct list* plist);    //打印链表的函数

int main()
{
struct list LIST;
LIST.head=NULL;
int num;
do{
    scanf("%d",&num);
    if(num!=-1){
        add(&LIST,num);      
    }
}while(num!=-1);

print(&LIST);

scanf("%d",&num);            //链表查找
struct node *p;          
int isfound=0;
for(p=LIST.head;p;p=p->next){
    if(p->value==num){
        printf("找到了\n");
        isfound=1;
        break;
    }
}
if(!isfound){
        printf("没找到\n");
    }    

struct node* q;            //将找到的节点删除并重连链表
for(q=NULL,p=LIST.head;p;q=p,p=p->next){
    if(p->value==num){
        if(q){
            q->next=p->next;
        }else{
            LIST.head=p->next;
        }
        free(p);
        break;
    }
}

print(&LIST);        //检验是否删除

for(p=LIST.head;p;p=q){           //链表的清除
q=p->next;
free(p);
}
    return 0;
}

void add(struct list *plist,int num){         //给链表增加节点的函数
struct node *p=(struct node *)malloc(sizeof(struct node));
        p->value=num;
        p->next=NULL;
        struct node *last=plist->head;
        if(last){
            while(last->next){
                last=last->next;
            }
            last->next=p;
        }else{
            plist->head=p;
        }
}

void print(struct list* plist){      //链表的输出函数
    struct node *p;
    for(p=plist->head;p;p=p->next){
        printf("%d ",p->value);
    }
    printf("\n");
}
#### 数组实现静止链表：
#include <stdio.h>

#define MAXSIZE 1000
#define OK 1
#define ERROR 0
typedef int ElemType;
typedef int Status;
typedef struct 
{
    ElemType data;
    int cur;
}StaticLinkList[MAXSIZE]; //定义结构体数组，与下面同理
  //struct STRUCT{
          //int a;
          //int b;
  //   }a[10];    只需要把结构变量定义给一个数组，即可得到结构体数组
//typedef与#define不同，typedef属于c语句，不在预处理里，由编译器处理
//并不是完全的文本替换

Status InitList(StaticLinkList space);//初始化静止链表的cur
int Malloc_SSL(StaticLinkList space);//取备用链表第一个结点的下标，并且改变指向备用链表的头指针
Status ListInsert(StaticLinkList space,int i,ElemType e);//插入元素的操作(新加元素也用这个)
int ListLength(StaticLinkList space); //返回当前静止链表的长度
Status ListDelete(StaticLinkList space,int i);//删除元素的操作
void Free_SSL(StaticLinkList space,int k);//释放操作（把删除的元素变为备用链表的头结点）

int main()
{
    StaticLinkList space;
    InitList(space);
    for(int i=1;i<=5;i++)
    {
        space[i].data=i;
    }
    space[MAXSIZE-1].cur=1;
    space[0].cur=6;
    space[5].cur=0;      //注意初始化的时候还要改一下指针
    int p=MAXSIZE-1;
    for(int i=1;i<=5;i++)
    {
        printf("%d ",space[space[p].cur].data);
        p=space[p].cur;
    }
    printf("\n");
    ListInsert(space,3,999);
    printf("%d\n",space[space[2].cur]);
    ListDelete(space,3);
    p=MAXSIZE-1;
    for(int i=1;i<=5;i++)
    {
        printf("%d ",space[space[p].cur].data);
        p=space[p].cur;
    }
    printf("\n");
    return 0;
}

Status InitList(StaticLinkList space)  //初始化静止链表的cur
{
    for(int i=0;i<MAXSIZE;i++)
    {
        space[i].cur=i+1;
    }
    space[MAXSIZE-1].cur=0;
    return OK;
}

int Malloc_SSL(StaticLinkList space)  //取备用链表第一个结点的下标，并且改变指向备用链表的头指针
{
    int i=space[0].cur;
    if(space[0].cur)
    {
        space[0].cur=space[i].cur;
    }
    return i;
}

Status ListInsert(StaticLinkList space,int i,ElemType e)//插入元素的操作
{
    int k=MAXSIZE-1;  //k首先是最后一个元素的下标
    if(i<1||i>ListLength(space)+1)
    {
        return ERROR;
    }
    int j=Malloc_SSL(space);
    if(j)
    {
        space[j].data=e;
        for(int m=1;m<=i-1;m++)
        {
            k=space[k].cur;
        }
        space[j].cur=space[k].cur;
        space[k].cur=j;
        return OK;
    }
    return ERROR;
}

int ListLength(StaticLinkList space)  //返回当前静止链表的长度
{
    int j=0;
    int i=space[MAXSIZE-1].cur;
    while(i)
    {
        i=space[i].cur;
        j++;
    }
    return j;
}

Status ListDelete(StaticLinkList space,int i)//删除元素的操作
{
    if(i<1||i>ListLength(space))
    {
        return ERROR;
    }
    int k=MAXSIZE-1;
    for(int j=1;j<=i-1;j++)
    {
        k=space[k].cur;
    }
    int j=space[k].cur;
    space[k].cur=space[j].cur;
    Free_SSL(space,j);
    return OK;
}

void Free_SSL(StaticLinkList space,int k)//释放操作（把删除的元素变为备用链表的头结点）
{
    space[k].cur=space[0].cur;
    space[0].cur=k;
}
#### 栈的链式存储结构;
#include <stdio.h>
#include <stdlib.h>

#define OK 1
#define ERROR 0

typedef int ElemType;
typedef int Status;

typedef struct StackNode
{
    ElemType data;
    struct StackNode *next;
} StackNode,*LinkStackPtr;

typedef struct 
{
    LinkStackPtr top;
    int count;
} LinkStack;

Status Push(LinkStack *S,ElemType e);//进栈操作
Status Pop(LinkStack *S,ElemType *e);//出栈操作
int StackEmpty(LinkStack *S);//判断栈是否空的操作
Status ClearStack(LinkStack *S);//清空链栈的操作

int main()
{
    LinkStack StackTop;
    StackTop.top=NULL;
    StackTop.count=0;
    for(int i=1;i<=5;i++)
    {
        Push(&StackTop,i);
    }
    LinkStackPtr last=StackTop.top;
    while(last!=NULL)
    {
        printf("%d ",last->data);
        last=last->next;
    }
    printf("\n");
    int a=5;
    ElemType *e=&a;
    Pop(&StackTop,e);
    printf("%d\n",*e);
    last=StackTop.top;
    while(last!=NULL)
    {
        printf("%d ",last->data);
        last=last->next;
    }
    printf("\n");
    ClearStack(&StackTop);
    return 0;
}

Status Push(LinkStack *S,ElemType e)//进栈操作
{
    LinkStackPtr s=(LinkStackPtr)malloc(sizeof(StackNode));
    s->data=e;
    s->next=S->top;
    S->top=s;
    S->count++;
    return OK;
}

Status Pop(LinkStack *S,ElemType *e)//出栈操作
{
    if(StackEmpty(S))
    {
        return ERROR;
    }
    *e=S->top->data;
    LinkStackPtr p=S->top;
    S->top=S->top->next;
    S->count--;
    free(p);   
    return OK;
}

int StackEmpty(LinkStack *S)//判断栈是否空的操作
{
    if(S=NULL)
    {
        return OK;
    }
    return ERROR;
}

Status ClearStack(LinkStack *S)//清空链栈的操作
{
    while(S->top!=NULL)
    {
        LinkStackPtr p=S->top;
        S->top=S->top->next;
        free(p);
    }
    S->count=0;
    return OK;
}
## Week2
## Week3
