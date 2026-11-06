# Pointer to Function

Write a C++ program to create a function pointer and use it to call a function.

## Program

```cpp
#include <iostream>
using namespace std;

int add(int a, int b) {
    return a + b;
}

int main() {
    int (*functionPointer)(int, int);

    functionPointer = add;

    int result = functionPointer(10, 20);

    cout << "Sum = " << result << endl;

    return 0;
}
```

## Sample Output

```text
Sum = 30
```
