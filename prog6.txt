
#include <stdio.h>
#include <stdlib.h>
struct DoublyNode {
    int data;
    struct DoublyNode* next;
    struct DoublyNode* prev;
};
struct DoublyLinkedList {
    struct DoublyNode* head;
};

struct DoublyNode* createDoublyNode(int data) {
    struct DoublyNode* newNode = (struct DoublyNode*)malloc(sizeof(struct DoublyNode));
    newNode->data = data;
    newNode->next = NULL;
    newNode->prev = NULL;
    return newNode;
}

void initList(struct DoublyLinkedList* list) {
    list->head = NULL;
}

void insert_at_beginning(struct DoublyLinkedList* list, int data) {
    struct DoublyNode* newNode = createDoublyNode(data);

    if (list->head == NULL) {
        list->head = newNode;
    } else {
        newNode->next = list->head;
        list->head->prev = newNode;
        list->head = newNode;
    }
    printf("inserted at the beginning.\n");
}
void insert_after_key(struct DoublyLinkedList* list, int key, int data) {
    if (list->head == NULL) {
        printf("List is empty . key not found \n", key);
        return;
    }

    struct DoublyNode* current = list->head;
    while (current != NULL && current->data != key) {
        current = current->next;
    }

    if (current == NULL) {
        printf("key %d not found \n", key);
        return;
    }

    struct DoublyNode* newNode = createDoublyNode(data);
    newNode->next = current->next;
    newNode->prev = current;

    if (current->next != NULL) {
        current->next->prev = newNode;
    }
    current->next = newNode;

    printf("inserted after %d.\n", key);
}
void insert_at_end(struct DoublyLinkedList* list, int data) {
    struct DoublyNode* newNode = createDoublyNode(data);

    if (list->head == NULL) {
        list->head = newNode;
    } else {
        struct DoublyNode* current = list->head;
        while (current->next != NULL) {
            current = current->next;
        }
        current->next = newNode;
        newNode->prev = current;
    }
}

void traverse_forward(struct DoublyLinkedList* list) {
    if (list->head == NULL) {
        printf("List is empty.\n");
        return;
    }

    struct DoublyNode* current = list->head;
    printf("doubly linked List(forward): ");
    while (current != NULL) {
        printf("%d <-> ", current->data);
        current = current->next;
    }
    printf("NULL\n");
}

void freeList(struct DoublyLinkedList* list) {
    struct DoublyNode* current = list->head;
    struct DoublyNode* next;
    while (current != NULL) {
        next = current->next;
        free(current);
        current = next;
    }
    list->head = NULL;
}

int main() {
    struct DoublyLinkedList list;
    initList(&list);

    printf("doubly linked list Operations\n");

    printf("insert at end\n");
    insert_at_end(&list, 10);
    insert_at_end(&list, 20);
    insert_at_end(&list, 30);
    traverse_forward(&list);

    printf("Insert at beginning ");
    insert_at_beginning(&list, 5);
    traverse_forward(&list);

    printf("Insert after key\n");
    insert_after_key(&list, 20, 25);
    traverse_forward(&list);

    insert_after_key(&list, 10, 15);
    traverse_forward(&list);

    printf("Try inserting after non-existent key ");
    insert_after_key(&list, 100, 99);

    printf("final list\n");
    traverse_forward(&list);
    freeList(&list);

    return 0;
}
