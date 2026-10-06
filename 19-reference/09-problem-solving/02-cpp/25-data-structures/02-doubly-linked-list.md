# Doubly Linked List

A **doubly linked list** stores two links in each node:

```text
[prev | data | next]
```

Example:

```text
nullptr <- 10 <-> 20 <-> 30 -> nullptr
```

## 1. Node Structure

```cpp
struct Node {
    int data;
    Node* prev;
    Node* next;
};
```

## 2. Create a Doubly Linked List

```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* prev;
    Node* next;
};

int main() {
    Node* first = new Node{10, nullptr, nullptr};
    Node* second = new Node{20, nullptr, nullptr};
    Node* third = new Node{30, nullptr, nullptr};

    first->next = second;

    second->prev = first;
    second->next = third;

    third->prev = second;

    Node* current = first;

    while (current != nullptr) {
        cout << current->data << " ";
        current = current->next;
    }

    return 0;
}
```

### Output

```text
10 20 30
```

## 3. Forward Traversal

```cpp
void displayForward(Node* head) {
    Node* current = head;

    while (current != nullptr) {
        cout << current->data << " ";
        current = current->next;
    }

    cout << endl;
}
```

## 4. Backward Traversal

```cpp
void displayBackward(Node* tail) {
    Node* current = tail;

    while (current != nullptr) {
        cout << current->data << " ";
        current = current->prev;
    }

    cout << endl;
}
```

## 5. Insert at Beginning

```cpp
void insertBeginning(Node*& head, Node*& tail, int value) {
    Node* newNode = new Node{value, nullptr, head};

    if (head != nullptr) {
        head->prev = newNode;
    } else {
        tail = newNode;
    }

    head = newNode;
}
```

## 6. Insert at End

```cpp
void insertEnd(Node*& head, Node*& tail, int value) {
    Node* newNode = new Node{value, tail, nullptr};

    if (tail != nullptr) {
        tail->next = newNode;
    } else {
        head = newNode;
    }

    tail = newNode;
}
```

## 7. Delete a Node

```cpp
void deleteNode(Node*& head, Node*& tail, Node* node) {
    if (node == nullptr) {
        return;
    }

    if (node->prev != nullptr) {
        node->prev->next = node->next;
    } else {
        head = node->next;
    }

    if (node->next != nullptr) {
        node->next->prev = node->prev;
    } else {
        tail = node->prev;
    }

    delete node;
}
```

## Singly vs Doubly Linked List

|Feature|Singly|Doubly|
|---|---|---|
|Next pointer|Yes|Yes|
|Previous pointer|No|Yes|
|Forward traversal|Yes|Yes|
|Backward traversal|No|Yes|
|Memory usage|Lower|Higher|
|Node complexity|Simpler|More complex|
