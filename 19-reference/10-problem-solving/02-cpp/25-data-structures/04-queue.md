# Queue

A **queue** is a linear data structure that follows **FIFO**:

> First In, First Out

Example:

```text
Front                         Back
  ↓                             ↓
[10] -> [20] -> [30] -> [40]
```

The first inserted element is removed first.

## 1. Using `std::queue`

```cpp
#include <iostream>
#include <queue>
using namespace std;

int main() {
    queue<int> q;

    q.push(10);
    q.push(20);
    q.push(30);

    cout << q.front() << endl;
    cout << q.back() << endl;

    return 0;
}
```

### Output

```text
10
30
```

## 2. Push and Pop

```cpp
#include <iostream>
#include <queue>
using namespace std;

int main() {
    queue<int> q;

    q.push(10);
    q.push(20);
    q.push(30);

    while (!q.empty()) {
        cout << q.front() << " ";
        q.pop();
    }

    return 0;
}
```

### Output

```text
10 20 30
```

## 3. Queue Operations

```cpp
queue<int> q;

q.push(10);
q.push(20);

cout << q.front() << endl;
cout << q.back() << endl;
cout << q.size() << endl;

q.pop();

cout << q.front() << endl;
```

### Output

```text
10
20
2
20
```

## 4. Simple Service Queue

```cpp
#include <iostream>
#include <queue>
using namespace std;

int main() {
    queue<string> customers;

    customers.push("Customer 1");
    customers.push("Customer 2");
    customers.push("Customer 3");

    while (!customers.empty()) {
        cout << "Serving: " << customers.front() << endl;
        customers.pop();
    }

    return 0;
}
```

### Output

```text
Serving: Customer 1
Serving: Customer 2
Serving: Customer 3
```

## Queue Operations

|Operation|Complexity|
|---|--:|
|`push()`|O(1)|
|`pop()`|O(1)|
|`front()`|O(1)|
|`back()`|O(1)|
|`empty()`|O(1)|
|`size()`|O(1)|
