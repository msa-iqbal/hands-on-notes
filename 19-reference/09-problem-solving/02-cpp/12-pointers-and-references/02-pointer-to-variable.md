# Pointer to Variable

Write a C++ program to use a pointer to access and modify the value of a variable.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int number = 10;
    int* pointer = &number;

    cout << "Before modification: " << number << endl;

    *pointer = 50;

    cout << "After modification: " << number << endl;

    return 0;
}
```

## Sample Output

```text
Before modification: 10
After modification: 50
```
