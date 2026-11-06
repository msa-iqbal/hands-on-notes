# Swap with Temporary Variable

Write a C++ program to swap two numbers using a temporary variable.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int a, b, temp;

    cout << "Enter two numbers: ";
    cin >> a >> b;

    cout << "Before swapping: a = " << a
         << ", b = " << b << endl;

    temp = a;
    a = b;
    b = temp;

    cout << "After swapping: a = " << a
         << ", b = " << b << endl;

    return 0;
}
```

## Sample Output

```text
Enter two numbers: 10 20
Before swapping: a = 10, b = 20
After swapping: a = 20, b = 10
```
