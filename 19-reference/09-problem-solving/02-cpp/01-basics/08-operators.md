# Operators

Write a C++ program to demonstrate arithmetic, relational, logical, assignment, increment, and decrement operators.

## C++ Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 10;
    int b = 3;

    cout << "Arithmetic Operators:" << endl;
    cout << "a + b = " << a + b << endl;
    cout << "a - b = " << a - b << endl;
    cout << "a * b = " << a * b << endl;
    cout << "a / b = " << a / b << endl;
    cout << "a % b = " << a % b << endl;

    cout << "\nRelational Operators:" << endl;
    cout << boolalpha;
    cout << "a == b: " << (a == b) << endl;
    cout << "a != b: " << (a != b) << endl;
    cout << "a > b: " << (a > b) << endl;
    cout << "a < b: " << (a < b) << endl;

    cout << "\nLogical Operators:" << endl;
    cout << "(a > 5 && b < 5): " << (a > 5 && b < 5) << endl;
    cout << "(a > 5 || b > 5): " << (a > 5 || b > 5) << endl;
    cout << "!(a == b): " << !(a == b) << endl;

    cout << "\nAssignment Operator:" << endl;
    int c = a;
    c += b;
    cout << "c += b: " << c << endl;

    cout << "\nIncrement and Decrement:" << endl;
    int x = 5;

    cout << "x: " << x << endl;
    cout << "++x: " << ++x << endl;
    cout << "--x: " << --x << endl;

    return 0;
}
```

## Sample Output

```text
Arithmetic Operators:
a + b = 13
a - b = 7
a * b = 30
a / b = 3
a % b = 1

Relational Operators:
a == b: false
a != b: true
a > b: true
a < b: false

Logical Operators:
(a > 5 && b < 5): true
(a > 5 || b > 5): true
!(a == b): true

Assignment Operator:
c += b: 13

Increment and Decrement:
x: 5
++x: 6
--x: 5
```
