# Alphabetic Right Angle Triangle 02

Write a C++ program to print a right-angle triangle using increasing alphabetic characters in each row.

## Pattern

```text
A
AB
ABC
ABCD
ABCDE
```

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;

    cout << "Enter number of rows: ";
    cin >> n;

    for (int i = 1; i <= n; i++) {
        for (int j = 0; j < i; j++) {
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
AB
ABC
ABCD
ABCDE
```
