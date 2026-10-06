# Pointer to Pointer

Write a C++ program to demonstrate a pointer to a pointer.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int number = 100;

    int* pointer = &number;
    int** pointerToPointer = &pointer;

    cout << "Value of variable: " << number << endl;
    cout << "Value using pointer: " << *pointer << endl;
    cout << "Value using pointer to pointer: "
         << **pointerToPointer << endl;

    return 0;
}
```

## Sample Output

```text
Value of variable: 100
Value using pointer: 100
Value using pointer to pointer: 100
```
