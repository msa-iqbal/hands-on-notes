# Pointer Basics

Write a C++ program to declare a pointer, store the address of a variable, and access the variable using the pointer.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int number = 25;
    int* pointer = &number;

    cout << "Value of variable: " << number << endl;
    cout << "Address of variable: " << pointer << endl;
    cout << "Value using pointer: " << *pointer << endl;

    return 0;
}
```

## Sample Output

```text
Value of variable: 25
Address of variable: 0x7ffd1234abcd
Value using pointer: 25
```

> The memory address will be different on different systems and program executions.
