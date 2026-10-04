# Alphabetic Right Angle Triangle 04

Write a C++ program to print a right-angle triangle using alphabets in reverse order within each row.

## Pattern

```text
A
BA
CBA
DCBA
EDCBA
```

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;

    cout << "Enter number of rows: ";
    cin >> n;

    for (int i = 0; i < n; i++) {
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
A
BA
CBA
DCBA
EDCBA
```
