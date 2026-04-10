#include <stdio.h>
#include <stdlib.h>
struct Node {
    int data;
    struct Node* next;
};
struct Node* head = NULL;
void insert_at_beginning(int data){
    struct Node* newNode =(struct Node*)malloc(sizeof(struct Node));
    if(newNode == NULL){
        printf("memory allocation failed\n");
        return;
    }
    newNode->data = data;
    newNode->next = head;
    head = newNode;
}
void insert_at_end(int data) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    if(newNode == NULL) {
        printf("memory allocation failed\n");
        return;
    }
    newNode->data = data;
    newNode->next = NULL;
    if(head==NULL) {
        head=newNode;
        return;
    }
    struct Node* temp = head;
    while(temp->next != NULL)
        temp = temp->next;
    temp->next = newNode;
}
void insert_after_key(int key, int data) {
    struct Node* temp = head;
    while(temp!=NULL) {
        if(temp->data == key) {
            struct Node* newNode=(struct Node*)malloc(sizeof(struct Node));
            if(newNode == NULL){
                printf("Memory allocation failed\n");
                return;
            }
            newNode->data =data;
            newNode->next =temp->next;
            temp->next=newNode;
            return;
        }
        temp = temp->next;
    }
    printf("Key not found\n");
}
void traverse() {
    struct Node* temp = head;
    while(temp != NULL) {
        printf("%d -> ", temp->data);
        temp = temp->next;
    }
    printf("NULL\n");
}
int main() {
    insert_at_beginning(1);
    insert_at_beginning(2);
    insert_at_end(33);
    insert_after_key(2, 22);

    traverse();

    return 0;
}
