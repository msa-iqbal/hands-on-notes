# Right Angle Triangle 04

Write a C++ program to print a right-angle triangle with numbers in reverse order.

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
