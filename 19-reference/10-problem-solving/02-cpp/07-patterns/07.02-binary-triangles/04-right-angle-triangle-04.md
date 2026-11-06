# Binary Right Angle Triangle 04

Write a C++ program to print a right-angle triangle where each row contains the same binary digit.

## Pattern

```text
0
11
000
1111
00000
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
        int digit = (i % 2 == 0) ? 1 : 0;

        for (int j = 1; j <= i; j++) {
            cout << digit;
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
11
000
1111
00000
```
