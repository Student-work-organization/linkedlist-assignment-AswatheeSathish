#include <stdio.h>
#include <stdlib.h>
struct CircularNode {
    int data;
    struct CircularNode* next;
};
struct CircularLinkedList {
    struct CircularNode* last;
};
struct CircularNode* createNode(int data) {
    struct CircularNode* newNode = (struct CircularNode*)malloc(sizeof(struct CircularNode));
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
}
void initList(struct CircularLinkedList* list) {
    list->last = NULL;
}
void insert_at_end(struct CircularLinkedList* list, int data) {
    struct CircularNode* newNode = createNode(data);

    if (list->last == NULL) {
        newNode->next = newNode;
        list->last = newNode;
    } else {
        newNode->next = list->last->next;
        list->last->next = newNode;
        list->last = newNode;
    }
}
void delete_at_beginning(struct CircularLinkedList* list) {
    if (list->last == NULL) {
        printf("List is empty.\n");
        return;
    }

    struct CircularNode* first = list->last->next;
    if (first == list->last) {
        free(first);
        list->last = NULL;
    } else {
        list->last->next = first->next;
        free(first);
    }
}

void delete_node(struct CircularLinkedList* list, int data) {
    if (list->last == NULL) {
        printf("List is empty.\n");
        return;
    }

    struct CircularNode *curr = list->last->next;
    struct CircularNode *prev = list->last;
    if (curr->data == data) {
        delete_at_beginning(list);
        return;
    }
    do {
        if (curr->data == data) {
            if (curr == list->last) {
                prev->next = curr->next;
                list->last = prev;
            } else {
                prev->next = curr->next;
            }

            free(curr);
            return;
        }

        prev = curr;
        curr = curr->next;

    } while (curr != list->last->next);

    printf("Node not found.\n");
}

void traverse(struct CircularLinkedList* list) {
    if (list->last == NULL) {
        printf("List is empty.\n");
        return;
    }

    struct CircularNode* curr = list->last->next;

    printf("Circular Linked List: ");
    do {
        printf("%d -> ", curr->data);
        curr = curr->next;
    } while (curr != list->last->next);

    printf("(back to head)\n");
}

int main() {
    struct CircularLinkedList list;
    initList(&list);

    insert_at_end(&list, 10);
    insert_at_end(&list, 20);
    insert_at_end(&list, 30);
    insert_at_end(&list, 40);

    traverse(&list);

    delete_node(&list, 10);
    traverse(&list);
    return 0;
}
