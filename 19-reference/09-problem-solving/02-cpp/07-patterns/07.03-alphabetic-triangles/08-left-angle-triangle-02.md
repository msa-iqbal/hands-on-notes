# Alphabetic Left Angle Triangle 02

Write a C++ program to print a left-aligned right-angle triangle using increasing alphabets.

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
        for (int space = 1; space <= n - i; space++) {
            cout << " ";
        }

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
