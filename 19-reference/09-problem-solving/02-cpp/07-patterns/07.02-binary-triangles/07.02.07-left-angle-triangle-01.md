# Binary Left Angle Triangle 01

Write a C++ program to print a left-aligned right-angle triangle using `1`.

## Pattern

```text
    1
   11
  111
 1111
11111
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
            cout << 1;
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
   11
  111
 1111
11111
```
