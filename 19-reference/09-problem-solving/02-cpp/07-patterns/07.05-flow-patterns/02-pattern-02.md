# Flow Pattern 02

Write a C++ program to print a continuous sequence of numbers in reverse order within each row.

## Pattern

```text
1
21
321
4321
54321
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
1
21
321
4321
54321
```
