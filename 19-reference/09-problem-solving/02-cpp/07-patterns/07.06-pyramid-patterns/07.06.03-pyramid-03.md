# Pyramid Pattern 03

Write a C++ program to print a centered pyramid where each row contains the same number.

## Pattern

```text
    1
   222
  33333
 4444444
555555555
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
            cout << i;
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
   222
  33333
 4444444
555555555
```
