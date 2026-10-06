# Flow Pattern 03

Write a C++ program to print a triangular pattern of alternating `0` and `1`.

## Pattern

```text
1
01
101
0101
10101
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
            cout << (i + j) % 2;
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
01
101
0101
10101
```
