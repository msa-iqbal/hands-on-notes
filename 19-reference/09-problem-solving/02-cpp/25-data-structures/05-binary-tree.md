# Binary Tree

A **binary tree** is a tree data structure where each node can have at most two children:

```text
        10
       /  \
     20    30
    /  \
   40   50
```

Each node can have:

- Left child

- Right child

## 1. Binary Tree Node

```cpp
struct Node {
    int data;
    Node* left;
    Node* right;
};
```

## 2. Create a Binary Tree

```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
};

int main() {
    Node* root = new Node{10, nullptr, nullptr};

    root->left = new Node{20, nullptr, nullptr};
    root->right = new Node{30, nullptr, nullptr};

    root->left->left = new Node{40, nullptr, nullptr};
    root->left->right = new Node{50, nullptr, nullptr};

    cout << root->data << endl;

    return 0;
}
```

### Output

```text
10
```

## Tree Structure

```text
        10
       /  \
     20    30
    /  \
   40   50
```

## 3. Preorder Traversal

Preorder:

```text
Root -> Left -> Right
```

```cpp
void preorder(Node* root) {
    if (root == nullptr) {
        return;
    }

    cout << root->data << " ";

    preorder(root->left);
    preorder(root->right);
}
```

Output:

```text
10 20 40 50 30
```

## 4. Inorder Traversal

Inorder:

```text
Left -> Root -> Right
```

```cpp
void inorder(Node* root) {
    if (root == nullptr) {
        return;
    }

    inorder(root->left);

    cout << root->data << " ";

    inorder(root->right);
}
```

Output:

```text
40 20 50 10 30
```

## 5. Postorder Traversal

Postorder:

```text
Left -> Right -> Root
```

```cpp
void postorder(Node* root) {
    if (root == nullptr) {
        return;
    }

    postorder(root->left);
    postorder(root->right);

    cout << root->data << " ";
}
```

Output:

```text
40 50 20 30 10
```

## 6. Level Order Traversal

Level order visits nodes level by level.

```cpp
#include <iostream>
#include <queue>
using namespace std;

void levelOrder(Node* root) {
    if (root == nullptr) {
        return;
    }

    queue<Node*> q;
    q.push(root);

    while (!q.empty()) {
        Node* current = q.front();
        q.pop();

        cout << current->data << " ";

        if (current->left != nullptr) {
            q.push(current->left);
        }

        if (current->right != nullptr) {
            q.push(current->right);
        }
    }
}
```

Output:

```text
10 20 30 40 50
```

## Important Terms

|Term|Meaning|
|---|---|
|Root|Top node|
|Parent|Node having child nodes|
|Child|Node connected below a parent|
|Leaf|Node without children|
|Edge|Connection between nodes|
|Height|Longest downward path from a node|
|Subtree|A tree contained within another tree|
