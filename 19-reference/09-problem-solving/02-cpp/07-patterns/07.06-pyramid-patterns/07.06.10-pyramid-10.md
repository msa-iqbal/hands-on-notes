# Pyramid Pattern 10

Write a C++ program to print a centered pyramid with increasing and decreasing numbers.

## Pattern

```text
    1
   12321
  1234321
 123454321
12345654321
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
            cout << j;
        }

        for (int j = i - 1; j >= 1; j--) {
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
   12321
  1234321
 123454321
12345654321
```
