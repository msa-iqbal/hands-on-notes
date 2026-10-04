# Binary Right Angle Triangle 05

Write a C++ program to print a right-angle triangle using alternating binary digits, starting each row with `0`.

## Pattern

```text
0
01
010
0101
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
        for (int j = 1; j <= i; j++) {
            cout << ((j - 1) % 2);
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
01
010
0101
01010
```
