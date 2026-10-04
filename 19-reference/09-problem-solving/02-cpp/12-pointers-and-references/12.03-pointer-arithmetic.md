# Pointer Arithmetic

Write a C++ program to access array elements using pointer arithmetic.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int numbers[] = {10, 20, 30, 40, 50};

    int* pointer = numbers;

    cout << "Array elements using pointer arithmetic:" << endl;

    for (int i = 0; i < 5; i++) {
        cout << *(pointer + i) << " ";
    }

    cout << endl;

    return 0;
}
```

## Sample Output

```text
Array elements using pointer arithmetic:
10 20 30 40 50
```
