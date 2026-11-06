# Pyramid Pattern 06

Write a C++ program to print a centered pyramid with descending numbers on each row.

## Pattern

```text
    1
   212
  32123
 4321234
543212345
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

        for (int j = i; j >= 1; j--) {
            cout << j;
        }

        for (int j = 2; j <= i; j++) {
            cout << j;
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
   212
  32123
 4321234
543212345
```
