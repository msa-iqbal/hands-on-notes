# Pyramid Pattern 16

Write a C++ program to print a centered inverted pyramid using `*`.

## Pattern

```text
*********
 *******
  *****
   ***
    *
```

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;

    cout << "Enter number of rows: ";
    cin >> n;

    for (int i = n; i >= 1; i--) {
        for (int space = 1; space <= n - i; space++) {
            cout << " ";
        }

        for (int j = 1; j <= 2 * i - 1; j++) {
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
*********
 *******
  *****
   ***
    *
```
