栈是一种先进后出的数据类型,只能操作栈顶,使用链表的头插法可以很快写出栈

输入:1-2-3-4
输出:4-3-2-1

```
//  Copyright © 2019 zhjzjnb. All rights reserved.

#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <assert.h>



typedef int Data;

typedef struct Node{
    Data data;
    struct Node *next;
}Node;

typedef struct Stack{
    Node *head;
    int top;
}Stack;

//创建一个空栈
Stack *create(){
    Stack *stack = malloc(sizeof(Stack));
    stack->head = NULL;
    stack->top = 0;
    return stack;
}

//向栈顶压入数据
void push(Stack *stack, Node *node){
    assert(stack!=NULL);
    assert(node!=NULL);
    Node *head = stack->head;
    stack->top++;
    if (head==NULL) {
        stack->head = node;
        node->next = NULL;
    }else{
        node->next = head;
        stack->head = node;
    }
}

//从栈顶弹出数据
Node *pop(Stack *stack){
    assert(stack!=NULL);
    if (stack->top==0) {
        return NULL;
    }
    stack->top--;
    Node *head = stack->head;
    stack->head = head->next;
    return head;
}

// 清理栈
void cleanup(Stack **stack){
    assert(*stack!=NULL);
    
    Node *head = (*stack)->head;
    Node *tmp;
    while (head) {
        tmp = head;
        head = head->next;
        //        printf("free data:%d\n",tmp->data);
        free(tmp);
    }
    (*stack)->head = NULL;
    (*stack)->top = 0;
    *stack = NULL;
    free(*stack);
}
// 打印栈
void dump(Stack *stack){
    assert(stack!=NULL);
    if (stack->top<=0) {
        printf("0 len stack\n");
        return;
    }
    Node *head = stack->head;
    printf("dump begin\n");
    while (head) {
        printf("%d ",head->data);
        head = head->next;
    }
    printf("\ndump end\n");
}

int main(int argc, const char * argv[]) {
    Stack *stack = create();
    
    for (int i=0; i<10; i++) {
        Node *node = malloc(sizeof(Node));
        node->data = i;
        push(stack,node);
    }
    
    dump(stack);
    
    Node *node = pop(stack);
    if(node){
        printf("pop success data:%d\n",node->data);
        free(node);
    }else{
        printf("pop failed\n");
    }

    dump(stack);
    cleanup(&stack);
    
    return 0;
}
```