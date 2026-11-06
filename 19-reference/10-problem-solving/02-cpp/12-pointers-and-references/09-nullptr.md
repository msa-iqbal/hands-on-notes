# nullptr

Write a C++ program to demonstrate the use of `nullptr` with a pointer and safely check whether the pointer contains a valid address.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int* pointer = nullptr;

    if (pointer == nullptr) {
        cout << "Pointer does not point to any object." << endl;
    }

    int number = 50;
    pointer = &number;

    if (pointer != nullptr) {
        cout << "Pointer is valid." << endl;
        cout << "Value = " << *pointer << endl;
    }

    return 0;
}
```

## Sample Output

```text
Pointer does not point to any object.
Pointer is valid.
Value = 50
```
