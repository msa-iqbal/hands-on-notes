# Pyramid Pattern 09

Write a C++ program to print a centered pyramid where each row contains the same alphabetic character.

## Pattern

```text
    A
   BBB
  CCCCC
 DDDDDDD
EEEEEEEEE
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

        for (int j = 0; j < 2 * i + 1; j++) {
            cout << char('A' + i);
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
   BBB
  CCCCC
 DDDDDDD
EEEEEEEEE
```
