# Priority Queue

Write a C++ program to demonstrate an STL `priority_queue`.

## Program

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

    cout << "Priority queue elements: ";

    while (!numbers.empty()) {
        cout << numbers.top() << " ";
        numbers.pop();
    }

    cout << endl;

    return 0;
}
```

## Sample Output

```text
Priority queue elements: 50 30 20 10
```
