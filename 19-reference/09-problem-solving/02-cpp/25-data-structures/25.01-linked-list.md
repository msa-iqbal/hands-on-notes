# Linked List

A **linked list** is a linear data structure where each element is stored in a node. Each node contains data and a pointer to the next node.

```text
[10 | next] -> [20 | next] -> [30 | nullptr]
```

## 1. Basic Node

```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* next;
};

int main() {
    Node* first = new Node{10, nullptr};

    cout << first->data << endl;

    delete first;

    return 0;
}
```

### Output

```text
10
```

## 2. Create Multiple Nodes

```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* next;
};

int main() {
    Node* first = new Node{10, nullptr};
    Node* second = new Node{20, nullptr};
    Node* third = new Node{30, nullptr};

    first->next = second;
    second->next = third;

    Node* current = first;

    while (current != nullptr) {
        cout << current->data << " ";
        current = current->next;
    }

    cout << endl;

    delete first;
    delete second;
    delete third;

    return 0;
}
```

### Output

```text
10 20 30
```

## 3. Traverse a Linked List

```cpp
void display(Node* head) {
    Node* current = head;

    while (current != nullptr) {
        cout << current->data << " ";
        current = current->next;
    }

    cout << endl;
}
```

## 4. Insert at Beginning

```cpp
void insertBeginning(Node*& head, int value) {
    Node* newNode = new Node{value, head};
    head = newNode;
}
```

Example:

```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* next;
};

void insertBeginning(Node*& head, int value) {
    Node* newNode = new Node{value, head};
    head = newNode;
}

void display(Node* head) {
    while (head != nullptr) {
        cout << head->data << " ";
        head = head->next;
    }
}

int main() {
    Node* head = nullptr;

    insertBeginning(head, 30);
    insertBeginning(head, 20);
    insertBeginning(head, 10);

    display(head);

    return 0;
}
```

### Output

```text
10 20 30
```

## 5. Insert at End

```cpp
void insertEnd(Node*& head, int value) {
    Node* newNode = new Node{value, nullptr};

    if (head == nullptr) {
        head = newNode;
        return;
    }

    Node* current = head;

    while (current->next != nullptr) {
        current = current->next;
    }

    current->next = newNode;
}
```

## 6. Search for a Value

```cpp
bool search(Node* head, int value) {
    while (head != nullptr) {
        if (head->data == value) {
            return true;
        }

        head = head->next;
    }

    return false;
}
```

Example:

```cpp
if (search(head, 20)) {
    cout << "Found";
} else {
    cout << "Not Found";
}
```

## 7. Delete First Node

```cpp
void deleteBeginning(Node*& head) {
    if (head == nullptr) {
        return;
    }

    Node* temp = head;
    head = head->next;

    delete temp;
}
```

## 8. Delete by Value

```cpp
void deleteValue(Node*& head, int value) {
    if (head == nullptr) {
        return;
    }

    if (head->data == value) {
        Node* temp = head;
        head = head->next;
        delete temp;
        return;
    }

    Node* current = head;

    while (current->next != nullptr &&
           current->next->data != value) {
        current = current->next;
    }

    if (current->next != nullptr) {
        Node* temp = current->next;
        current->next = temp->next;
        delete temp;
    }
}
```

## Key Points

|Operation|Typical Complexity|
|---|--:|
|Access by index|O(n)|
|Search|O(n)|
|Insert at beginning|O(1)|
|Delete beginning|O(1)|
|Insert at end|O(n)|
|Delete by value|O(n)|
