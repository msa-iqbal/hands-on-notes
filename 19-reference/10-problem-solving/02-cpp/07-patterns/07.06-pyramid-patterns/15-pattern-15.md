# Pyramid Pattern 15

Write a C++ program to print a hollow centered pyramid.

## Pattern

```text
    *
   * *
  *   *
 *     *
*********
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

        if (i == 1) {
            cout << "*";
        } else if (i == n) {
            for (int j = 1; j <= 2 * n - 1; j++) {
                cout << "*";
            }
        } else {
            cout << "*";

            for (int space = 1; space <= 2 * i - 3; space++) {
                cout << " ";
            }

            cout << "*";
        }

        cout << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter number of rows: 5
    *
   * *
  *   *
 *     *
*********
```
