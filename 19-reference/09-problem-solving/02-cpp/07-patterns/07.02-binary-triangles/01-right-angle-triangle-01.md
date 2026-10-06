# Binary Right Angle Triangle 01

Write a C++ program to print a right-angle triangle using `1` in every position.

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
