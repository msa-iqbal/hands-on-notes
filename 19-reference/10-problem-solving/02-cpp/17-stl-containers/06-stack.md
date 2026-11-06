# Stack

Write a C++ program to demonstrate an STL `stack`.

## Program

```cpp
#include <iostream>
#include <stack>
using namespace std;

int main() {
    stack<int> numbers;

    numbers.push(10);
    numbers.push(20);
    numbers.push(30);

    cout << "Top element: " << numbers.top() << endl;

    cout << "Stack elements: ";

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
Top element: 30
Stack elements: 30 20 10
```
