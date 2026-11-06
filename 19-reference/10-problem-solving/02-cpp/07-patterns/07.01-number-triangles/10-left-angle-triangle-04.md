# Left Angle Triangle 04

Write a C++ program to print a left-aligned right-angle triangle with numbers in reverse order.

## Pattern

```text
54321
 4321
  321
   21
    1
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

        for (int j = i; j >= 1; j--) {
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
54321
 4321
  321
   21
    1
```
