# Binary Left Angle Triangle 06

Write a C++ program to print a left-aligned right-angle triangle using binary digits with an alternating starting digit.

## Pattern

```text
    0
   10
  010
 1010
01010
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

        for (int j = 1; j <= i; j++) {
            cout << ((i + j + 1) % 2);
        }

        cout << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter number of rows: 5
    0
   10
  010
 1010
01010
```
