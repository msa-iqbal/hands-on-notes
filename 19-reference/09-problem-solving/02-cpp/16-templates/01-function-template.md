# Function Template

Write a C++ program to create a function template that can work with different data types.

## Program

```cpp
#include <iostream>
using namespace std;

template <typename T>
T add(T a, T b) {
    return a + b;
}

int main() {
    cout << "Integer sum: " << add(10, 20) << endl;
    cout << "Double sum: " << add(10.5, 20.5) << endl;

    return 0;
}
```

## Sample Output

```text
Integer sum: 30
Double sum: 31
```
