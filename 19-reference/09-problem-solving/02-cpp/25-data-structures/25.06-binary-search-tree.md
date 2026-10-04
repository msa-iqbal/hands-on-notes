# Binary Search Tree

A **Binary Search Tree (BST)** is a binary tree that maintains an ordering rule:

```text
Left subtree < Root < Right subtree
```

Example:

```text
        50
       /  \
     30    70
    / \    / \
   20 40  60 80
```

## 1. Node Structure

```cpp
struct Node {
    int data;
    Node* left;
    Node* right;
};
```

## 2. Create a Node

```cpp
Node* createNode(int value) {
    return new Node{value, nullptr, nullptr};
}
```

## 3. Insert into BST

```cpp
Node* insert(Node* root, int value) {
    if (root == nullptr) {
        return createNode(value);
    }

    if (value < root->data) {
        root->left = insert(root->left, value);
    } else if (value > root->data) {
        root->right = insert(root->right, value);
    }

    return root;
}
```

## 4. Complete Insert Example

```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
};

Node* createNode(int value) {
    return new Node{value, nullptr, nullptr};
}

Node* insert(Node* root, int value) {
    if (root == nullptr) {
        return createNode(value);
    }

    if (value < root->data) {
        root->left = insert(root->left, value);
    } else if (value > root->data) {
        root->right = insert(root->right, value);
    }

    return root;
}

void inorder(Node* root) {
    if (root == nullptr) {
        return;
    }

    inorder(root->left);
    cout << root->data << " ";
    inorder(root->right);
}

int main() {
    Node* root = nullptr;

    root = insert(root, 50);
    root = insert(root, 30);
    root = insert(root, 70);
    root = insert(root, 20);
    root = insert(root, 40);
    root = insert(root, 60);
    root = insert(root, 80);

    inorder(root);

    return 0;
}
```

### Output

```text
20 30 40 50 60 70 80
```

## 5. Search

```cpp
bool search(Node* root, int value) {
    if (root == nullptr) {
        return false;
    }

    if (root->data == value) {
        return true;
    }

    if (value < root->data) {
        return search(root->left, value);
    }

    return search(root->right, value);
}
```

Example:

```cpp
if (search(root, 60)) {
    cout << "Found";
} else {
    cout << "Not Found";
}
```

### Output

```text
Found
```

## 6. Find Minimum

```cpp
Node* findMin(Node* root) {
    while (root != nullptr && root->left != nullptr) {
        root = root->left;
    }

    return root;
}
```

## 7. Find Maximum

```cpp
Node* findMax(Node* root) {
    while (root != nullptr && root->right != nullptr) {
        root = root->right;
    }

    return root;
}
```

## 8. Delete a Node

```cpp
Node* deleteNode(Node* root, int value) {
    if (root == nullptr) {
        return nullptr;
    }

    if (value < root->data) {
        root->left = deleteNode(root->left, value);
    } else if (value > root->data) {
        root->right = deleteNode(root->right, value);
    } else {
        if (root->left == nullptr) {
            Node* temp = root->right;
            delete root;
            return temp;
        }

        if (root->right == nullptr) {
            Node* temp = root->left;
            delete root;
            return temp;
        }

        Node* temp = findMin(root->right);

        root->data = temp->data;
        root->right = deleteNode(root->right, temp->data);
    }

    return root;
}
```

## Complexity

For a reasonably balanced BST:

|Operation|Average/Typical|Worst Case|
|---|--:|--:|
|Search|O(log n)|O(n)|
|Insert|O(log n)|O(n)|
|Delete|O(log n)|O(n)|

The worst case occurs when the tree becomes highly skewed.