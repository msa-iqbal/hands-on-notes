# Pyramid Pattern 07

Write a C++ program to print a centered pyramid of alternating `1` and `0`.

## Pattern

```text
    1
   101
  10101
 1010101
101010101
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

        for (int j = 1; j <= 2 * i - 1; j++) {
            cout << (j % 2);
        }

        cout << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter number of rows: 5
    1
   101
  10101
 1010101
101010101
```
