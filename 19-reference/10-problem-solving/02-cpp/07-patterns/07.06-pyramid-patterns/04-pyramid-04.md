# Pyramid Pattern 04

Write a C++ program to print a centered pyramid using consecutive numbers.

## Pattern

```text
    1
   234
  56789
 1011121314
1516171819202123
```

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;
    int number = 1;

    cout << "Enter number of rows: ";
    cin >> n;

    for (int i = 1; i <= n; i++) {
        for (int space = 1; space <= n - i; space++) {
            cout << " ";
        }

        for (int j = 1; j <= 2 * i - 1; j++) {
            cout << number;
            number++;
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
   234
  56789
 1011121314
151617181920212223
```
