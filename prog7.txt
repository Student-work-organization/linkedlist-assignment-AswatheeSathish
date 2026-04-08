#include <stdio.h>
#include <stdlib.h>
struct DoublyNode {
    int data;
    struct DoublyNode* next;
    struct DoublyNode* prev;
};
struct DoublyLinkedList {
    struct DoublyNode* head;
    struct DoublyNode* tail;
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
    list->tail = NULL;
}
void delete_node(struct DoublyLinkedList* list, int data) {
    if (list->head == NULL) {
        printf("list is empty.\n");
        return;
    }

    struct DoublyNode* current = list->head;
    while (current != NULL && current->data != data) {
        current = current->next;
    }

    if (current == NULL) {
        printf("node with data not found.\n");
        return;
    }
    if (current == list->head) {
        list->head = current->next;
        if (list->head != NULL)
            list->head->prev = NULL;
        else
            list->tail = NULL;
    }
    else if (current == list->tail) {
        list->tail = current->prev;
        list->tail->next = NULL;
    }

    else {
        current->prev->next = current->next;
        current->next->prev = current->prev;
    }

    printf("deleted first node with data %d.\n", current->data);
    free(current);
}

void delete_the_end_node(struct DoublyLinkedList* list) {
    if (list->tail == NULL) {
        printf("list is empty.\n");
        return;
    }

    struct DoublyNode* temp = list->tail;

    if (list->head == list->tail) {
        list->head = NULL;
        list->tail = NULL;
    } else {
        list->tail = list->tail->prev;
        list->tail->next = NULL;
    }

    printf("deleted end node with data %d.\n", temp->data);
    free(temp);
}
void delete_node_with_givenData(struct DoublyLinkedList* list, int data) {
    if (list->head == NULL) {
        printf("list is empty.\n");
        return;
    }

    struct DoublyNode* current = list->head;
    int count = 0;

    while (current != NULL) {
        struct DoublyNode* nextNode = current->next;

        if (current->data == data) {

            if (current == list->head) {
                list->head = current->next;
                if (list->head != NULL)
                    list->head->prev = NULL;
                else
                    list->tail = NULL;
            }
            else if (current == list->tail) {
                list->tail = current->prev;
                list->tail->next = NULL;
            }
            else {
                current->prev->next = current->next;
                current->next->prev = current->prev;
            }
            free(current);
            count++;
        }
        current = nextNode;
    }

    if (count > 0)
        printf("deleted %d node with data %d.\n", count, data);
    else
        printf("no node with data %d found.\n", data);
}
void insert_after_key(struct DoublyLinkedList* list, int key, int data) {
    if (list->head == NULL) {
        printf("list is empty. Key %d not found.\n", key);
        return;
    }

    struct DoublyNode* current = list->head;
    while (current != NULL && current->data != key) {
        current = current->next;
    }

    if (current == NULL) {
        printf("key %d not found.\n", key);
        return;
    }

    struct DoublyNode* newNode = createDoublyNode(data);
    newNode->next = current->next;
    newNode->prev = current;

    if (current->next != NULL) {
        current->next->prev = newNode;
    } else {
        list->tail = newNode;
    }
    current->next = newNode;

    printf("inserted %d after %d.\n", data, key);
}
void traverse_backward(struct DoublyLinkedList* list) {
    if (list->tail == NULL) {
        printf("list is empty.\n");
        return;
    }

    struct DoublyNode* current = list->tail;
    printf("doubly Linked List (backward): ");
    while (current != NULL) {
        printf("%d <-> ", current->data);
        current = current->prev;
    }
    printf("NULL\n");
}

void traverse_forward(struct DoublyLinkedList* list) {
    if (list->head == NULL) {
        printf("List is empty.\n");
        return;
    }

    struct DoublyNode* current = list->head;
    printf("doubly linked list (forward):  ");
    while (current != NULL) {
        printf("%d <-> ", current->data);
        current = current->next;
    }
    printf("NULL\n");
}

void freeList(struct DoublyLinkedList* list) {
    struct DoublyNode* current = list->head;
    while (current != NULL) {
        struct DoublyNode* temp = current;
        current = current->next;
        free(temp);
    }
    list->head = NULL;
    list->tail = NULL;
}
int main() {
    struct DoublyLinkedList list;
    initList(&list);


    insert_after_key(&list, -1, 10);
    printf("Creating initial list: 1, 20, 33, 20, 44\n");
    struct DoublyNode* n1 = createDoublyNode(1);
    struct DoublyNode* n2 = createDoublyNode(20);
    struct DoublyNode* n3 = createDoublyNode(33);
    struct DoublyNode* n4 = createDoublyNode(20);
    struct DoublyNode* n5 = createDoublyNode(44);

    list.head = n1;
    n1->next = n2; n2->prev = n1;
    n2->next = n3; n3->prev = n2;
    n3->next = n4; n4->prev = n3;
    n4->next = n5; n5->prev = n4;
    list.tail = n5;

    traverse_forward(&list);
    traverse_backward(&list);

    printf("delete first node with data 20 \n");
    delete_node(&list, 20);
    traverse_forward(&list);
    traverse_backward(&list);

    printf("Delete last node\n");
    delete_the_end_node(&list);
    traverse_forward(&list);
    traverse_backward(&list);

    printf(" delete node with givenData(20)\n");
    delete_node_with_givenData(&list, 20);
    traverse_forward(&list);
    traverse_backward(&list);
    freeList(&list);

    return 0;
}
