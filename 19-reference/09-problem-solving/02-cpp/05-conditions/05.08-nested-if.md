# Nested If

Write a C++ program using nested `if` statements to determine the largest among three numbers.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int a, b, c;

    cout << "Enter three integers: ";
    cin >> a >> b >> c;

    if (a >= b) {
        if (a >= c) {
            cout << "Largest number = " << a << endl;
        } else {
            cout << "Largest number = " << c << endl;
        }
    } else {
        if (b >= c) {
            cout << "Largest number = " << b << endl;
        } else {
            cout << "Largest number = " << c << endl;
        }
    }

    return 0;
}
```

## Sample Output

```text
Enter three integers: 35 72 48
Largest number = 72
```
