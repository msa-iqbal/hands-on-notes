# Pyramid Pattern 12

Write a C++ program to print a centered pyramid using consecutive alphabetic characters.

## Pattern

```text
    A
   BCD
  EFGHI
 JKLMNOP
QRSTUVWXY
```

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;
    char ch = 'A';

    cout << "Enter number of rows: ";
    cin >> n;

    for (int i = 1; i <= n; i++) {
        for (int space = 1; space <= n - i; space++) {
            cout << " ";
        }

        for (int j = 1; j <= 2 * i - 1; j++) {
            cout << ch;

            ch++;

            if (ch > 'Z') {
                ch = 'A';
            }
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
   BCD
  EFGHI
 JKLMNOP
QRSTUVWXY
```
