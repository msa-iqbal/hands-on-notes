# Pointer vs Reference

Write a C++ program to demonstrate the basic difference between a pointer and a reference.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int number = 100;

    int* pointer = &number;
    int& reference = number;

    cout << "Original value: " << number << endl;
    cout << "Value using pointer: " << *pointer << endl;
    cout << "Value using reference: " << reference << endl;

    *pointer = 200;

    cout << "After modifying through pointer: "
         << number << endl;

    reference = 300;

    cout << "After modifying through reference: "
         << number << endl;

    return 0;
}
```

## Sample Output

```text
Original value: 100
Value using pointer: 100
Value using reference: 100
After modifying through pointer: 200
After modifying through reference: 300
```
