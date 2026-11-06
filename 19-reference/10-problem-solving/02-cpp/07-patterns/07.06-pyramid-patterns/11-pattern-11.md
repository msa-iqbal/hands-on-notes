# Pyramid Pattern 11

Write a C++ program to print a centered pyramid with descending and ascending alphabets.

## Pattern

```text
    A
   BAB
  CBABC
 DCBABCD
EDCBABCDE
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
        for (int space = 0; space < n - i - 1; space++) {
            cout << " ";
        }

        for (int j = i; j >= 0; j--) {
            cout << char('A' + j);
        }

        for (int j = 1; j <= i; j++) {
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
   BAB
  CBABC
 DCBABCD
EDCBABCDE
```
