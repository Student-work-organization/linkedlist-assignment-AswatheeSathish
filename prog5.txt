#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    struct Node* next;
};
struct Node* createNode(int data) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
}
void delete_from_the_beginning(struct Node** head) {
    if (*head == NULL) {
        printf("list is empty.");
        return;
    }
    struct Node* temp = *head;
    *head = (*head)->next;
    printf("deleted node with data %d from beginning.\n", temp->data);
    free(temp);
}
void delete_the_end_node(struct Node** head) {
    if (*head == NULL) {
        printf("list is empty.\n");
        return;
    }
    if ((*head)->next == NULL) {
        printf("Deleted node with data %d from end.\n", (*head)->data);
        free(*head);
        *head = NULL;
        return;
    }

    struct Node* current = *head;
    while (current->next->next != NULL) {
        current = current->next;
    }
    printf("deleted node with data %d from end.\n", current->next->data);
    free(current->next);
    current->next = NULL;
}
void delete_node_with_givenData(struct Node** head, int data) {
    if (*head == NULL) {
        printf("list is empty\n");
        return;
    }
    if ((*head)->data == data) {
        struct Node* temp = *head;
        *head = (*head)->next;
        printf("deleted node with data %d.\n", temp->data);
        free(temp);
        return;
    }

    struct Node* current = *head;
    while (current->next != NULL && current->next->data != data) {
        current = current->next;
    }
    if (current->next == NULL) {
        printf("node with data not found.\n");
        return;
    }
    struct Node* temp = current->next;
    current->next = current->next->next;
    printf("deleted node with data %d.\n", temp->data);
    free(temp);
}
void insert_after_key(struct Node** head, int key, int data) {
    struct Node* current = *head;
    while (current != NULL && current->data != key) {
        current = current->next;
    }
    if (current == NULL) {
        printf("key %d not found.\n", key);
        return;
    }
    struct Node* newNode = createNode(data);
    newNode->next = current->next;
    current->next = newNode;
    printf("inserted %d after %d.\n", data, key);
}

void traverse(struct Node* head) {
    if (head == NULL) {
        printf("List is empty.\n");
        return;
    }
    struct Node* current = head;
    printf("Linked List: ");
    while (current != NULL) {
        printf("%d -> ", current->data);
        current = current->next;
    }
    printf("NULL\n");
}
int main() {
    struct Node* head = NULL;
    head = createNode(1);
    head->next = createNode(20);
    head->next->next = createNode(3);
    head->next->next->next = createNode(44);

    printf("list ");
    traverse(head);

    printf("delete from beginning\n");
    delete_from_the_beginning(&head);
    traverse(head);

    printf("delete end node \n");
    delete_the_end_node(&head);
    traverse(head);

    printf("delete node with data 20 \n");
    delete_node_with_givenData(&head, 20);
    traverse(head);

    printf("insert 25 after key 3 \n");
    insert_after_key(&head, 30, 25);
    traverse(head);

    printf("try deleting from empty list\n");
    delete_from_the_beginning(&head);
    delete_from_the_beginning(&head);
    traverse(head);

    return 0;
}
