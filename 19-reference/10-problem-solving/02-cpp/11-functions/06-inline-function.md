# Inline Function

Write a C++ program to demonstrate an inline function.

## Program

```cpp
#include <iostream>
using namespace std;

inline int square(int number) {
    return number * number;
}

int main() {
    int number;

    cout << "Enter a number: ";
    cin >> number;

    cout << "Square = " << square(number) << endl;

    return 0;
}
```

## Sample Output

```text
Enter a number: 8
Square = 64
```
