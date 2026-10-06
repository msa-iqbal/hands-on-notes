# Alphabetic Left Angle Triangle 04

Write a C++ program to print a left-aligned right-angle triangle with alphabets in reverse order.

## Pattern

```text
EDCBA
 DCBA
  CBA
   BA
    A
```

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;

    cout << "Enter number of rows: ";
    cin >> n;

    for (int i = n - 1; i >= 0; i--) {
        for (int space = 0; space < n - i - 1; space++) {
            cout << " ";
        }

        for (int j = i; j >= 0; j--) {
            cout << char('A' + j);
        }

        cout << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter number of rows: 5
EDCBA
 DCBA
  CBA
   BA
    A
```
