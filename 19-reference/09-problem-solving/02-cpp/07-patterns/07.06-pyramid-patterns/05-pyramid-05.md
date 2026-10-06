# Pyramid Pattern 05

Write a C++ program to print a centered pyramid using alphabets.

## Pattern

```text
    A
   ABC
  ABCDE
 ABCDEFG
ABCDEFGHI
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

        for (int j = 0; j < 2 * i - 1; j++) {
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
   ABC
  ABCDE
 ABCDEFG
ABCDEFGHI
```
