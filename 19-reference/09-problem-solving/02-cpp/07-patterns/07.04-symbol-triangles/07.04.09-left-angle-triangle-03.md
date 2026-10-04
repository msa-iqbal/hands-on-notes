# Symbol Left Angle Triangle 03

Write a C++ program to print a left-aligned right-angle triangle using the `$` symbol.

## Pattern

```text
    $
   $$
  $$$
 $$$$
$$$$$
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
            cout << ' ';
        }

        for (int j = 1; j <= i; j++) {
            cout << '$';
        }

        cout << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter number of rows: 5
    $
   $$
  $$$
 $$$$
$$$$$
```
