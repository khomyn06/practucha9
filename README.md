Практична робота №9 Хомин Михайло

#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int position;
    int step_length;
    int steps_count;
    struct Node* next;
} Node;

Node* createNode(int position, int step_length, int steps_count) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    newNode->position = position;
    newNode->step_length = step_length;
    newNode->steps_count = steps_count;
    newNode->next = NULL;
    return newNode;
}


void enqueue(Node** front, Node** rear, int position, int step_length, int steps_count) {
    Node* newNode = createNode(position, step_length, steps_count);
    if (*rear == NULL) {
        *front = *rear = newNode;
    } else {
        (*rear)->next = newNode;
        *rear = newNode;
    }
}

Node* dequeue(Node** front) {
    if (*front == NULL) return NULL;
    Node* temp = *front;
    *front = (*front)->next;
    return temp;
}


int minSteps(int x, int y) {
    if (x == y) return 0;

    Node* front = NULL;
    Node* rear = NULL;
    enqueue(&front, &rear, x, 1, 1);

    while (front != NULL) {
        Node* current = dequeue(&front);

        int position = current->position;
        int step_length = current->step_length;
        int steps_count = current->steps_count;
        free(current);

        if (position + step_length == y || position + step_length + 1 == y) {
            return steps_count + 1;
        }

        if (position + step_length - 1 >= x) {
            enqueue(&rear, &rear, position + step_length - 1, step_length - 1, steps_count + 1);
        }
        enqueue(&rear, &rear, position + step_length, step_length, steps_count + 1);
        enqueue(&rear, &rear, position + step_length + 1, step_length + 1, steps_count + 1);
    }

    return -1;
}

int main() {
    int x, y;
    scanf("%d %d", &x, &y);
    printf("%d\n", minSteps(x, y));
    return 0;
}
