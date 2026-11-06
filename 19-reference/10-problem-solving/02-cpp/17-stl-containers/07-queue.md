# Queue

Write a C++ program to demonstrate an STL `queue`.

## Program

```cpp
#include <iostream>
#include <queue>
using namespace std;

int main() {
    queue<int> numbers;

    numbers.push(10);
    numbers.push(20);
    numbers.push(30);

    cout << "Front: " << numbers.front() << endl;
    cout << "Back: " << numbers.back() << endl;

    cout << "Queue elements: ";

    while (!numbers.empty()) {
        cout << numbers.front() << " ";
        numbers.pop();
    }

    cout << endl;

    return 0;
}
```

## Sample Output

```text
Front: 10
Back: 30
Queue elements: 10 20 30
```
