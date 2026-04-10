
#include <stdio.h>
#include <stdlib.h>
struct CircularNode {
    int data;
    struct CircularNode* next;
};
struct CircularLinkedList {
    struct CircularNode* last;
};
struct CircularNode* createCircularNode(int data) {
    struct CircularNode* newNode = (struct CircularNode*)malloc(sizeof(struct CircularNode));
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
}
void initList(struct CircularLinkedList* list) {
    list->last = NULL;
}
void insert_at_beginning(struct CircularLinkedList* list, int data) {
    struct CircularNode* newNode = createCircularNode(data);

    if (list->last == NULL) {
        newNode->next = newNode;
        list->last = newNode;
    } else {
        newNode->next = list->last->next;
        list->last->next = newNode;
    }
    printf("Inserted %d at the beginning.\n", data);
}
void traverse(struct CircularLinkedList* list) {
    if (list->last == NULL) {
        printf("List is empty.\n");
        return;
    }

    struct CircularNode* current = list->last->next;
    printf("Circular Linked List: ");
    do {
        printf("%d -> ", current->data);
        current = current->next;
    } while (current != list->last->next);
    printf("(back to head)\n");
}
void freeList(struct CircularLinkedList* list) {
    if (list->last == NULL) return;

    struct CircularNode* current = list->last->next;
    struct CircularNode* temp;

    while (current != list->last) {
        temp = current;
        current = current->next;
        free(temp);
    }
    free(list->last);
    list->last = NULL;
}
int main() {
    struct CircularLinkedList list;
    initList(&list);

    printf("circular Linked List \n");

    printf("insert at beginning\n");
    insert_at_beginning(&list, 1);
    traverse(&list);

    insert_at_beginning(&list, 220);
    traverse(&list);

    insert_at_beginning(&list, 32);
    traverse(&list);

    insert_at_beginning(&list, 44);
    traverse(&list);

    printf("final list\n");
    traverse(&list);
    freeList(&list);

    return 0;
}
