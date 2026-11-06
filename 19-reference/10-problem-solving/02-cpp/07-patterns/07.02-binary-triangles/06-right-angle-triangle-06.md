# Binary Right Angle Triangle 06

Write a C++ program to print a right-angle triangle using binary digits with the starting digit alternating between rows.

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
