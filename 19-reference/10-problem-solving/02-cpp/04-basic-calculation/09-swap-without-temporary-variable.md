# Swap without Temporary Variable

Write a C++ program to swap two numbers without using a temporary variable.

This example uses `std::swap()`, which is the idiomatic C++ approach.

## Program

```cpp
#include <iostream>
#include <utility>
using namespace std;

int main() {
    int a, b;

    cout << "Enter two numbers: ";
    cin >> a >> b;

    cout << "Before swapping: a = " << a
         << ", b = " << b << endl;

    swap(a, b);

    cout << "After swapping: a = " << a
         << ", b = " << b << endl;

    return 0;
}
```

## Sample Output

```text
Enter two numbers: 15 25
Before swapping: a = 15, b = 25
After swapping: a = 25, b = 15
```
