# Variadic Templates

Write a C++ program to demonstrate a variadic function template that can accept any number of arguments.

## Program

```cpp
#include <iostream>
using namespace std;

template <typename T>
void print(T value) {
    cout << value << endl;
}

template <typename T, typename... Args>
void print(T first, Args... rest) {
    cout << first << " ";
    print(rest...);
}

int main() {
    cout << "Values: ";

    print(10, 20, 30, 40, 50);

    return 0;
}
```

## Sample Output

```text
Values: 10 20 30 40 50
```
