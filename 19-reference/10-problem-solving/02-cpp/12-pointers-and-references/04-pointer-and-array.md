# Pointer and Array

Write a C++ program to access and display array elements using a pointer.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int numbers[5] = {10, 20, 30, 40, 50};

    int* pointer = numbers;

    cout << "Array elements:" << endl;

    for (int i = 0; i < 5; i++) {
        cout << "Element " << i + 1 << ": "
             << *(pointer + i) << endl;
    }

    return 0;
}
```

## Sample Output

```text
Array elements:
Element 1: 10
Element 2: 20
Element 3: 30
Element 4: 40
Element 5: 50
```
