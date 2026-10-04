# Heap

A **heap** is a tree-based data structure commonly represented using an array.

Two common types are:

- **Max Heap** — largest element at the top
- **Min Heap** — smallest element at the top

A heap data structure is different from **heap memory** used for dynamic allocation.

## 1. Max Heap with `priority_queue`

C++ provides a max heap through `std::priority_queue`.

```cpp
#include <iostream>
#include <queue>
using namespace std;

int main() {
    priority_queue<int> numbers;

    numbers.push(30);
    numbers.push(10);
    numbers.push(50);
    numbers.push(20);

    cout << numbers.top() << endl;

    return 0;
}
```

### Output

```text
50
```

## 2. Remove Elements

```cpp
#include <iostream>
#include <queue>
using namespace std;

int main() {
    priority_queue<int> numbers;

    numbers.push(30);
    numbers.push(10);
    numbers.push(50);
    numbers.push(20);

    while (!numbers.empty()) {
        cout << numbers.top() << " ";
        numbers.pop();
    }

    return 0;
}
```

### Output

```text
50 30 20 10
```

## 3. Min Heap

Use `greater<int>` to create a min heap.

```cpp
#include <iostream>
#include <queue>
using namespace std;

int main() {
    priority_queue<int, vector<int>, greater<int>> numbers;

    numbers.push(30);
    numbers.push(10);
    numbers.push(50);
    numbers.push(20);

    while (!numbers.empty()) {
        cout << numbers.top() << " ";
        numbers.pop();
    }

    return 0;
}
```

### Output

```text
10 20 30 50
```

## 4. Heap Representation

A binary heap can be stored in an array.

For a zero-based array:

```text
Parent:
(i - 1) / 2

Left child:
2 * i + 1

Right child:
2 * i + 2
```

Example:

```text
        50
       /  \
     30    40
    / \
   10 20
```

Array:

```text
[50, 30, 40, 10, 20]
```

## 5. Basic Heap Operations

|Operation|Complexity|
|---|--:|
|Get top|O(1)|
|Insert|O(log n)|
|Remove top|O(log n)|
|Build heap|O(n)|

## Common Uses

- Priority queues
- Scheduling
- Finding top K elements
- Heap sort
- Graph algorithms
- Event processing
