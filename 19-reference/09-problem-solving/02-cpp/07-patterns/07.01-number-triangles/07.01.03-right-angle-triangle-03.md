# Right Angle Triangle 03

Write a C++ program to print a right-angle triangle where each row contains the same number.

## Pattern

```text
1
22
333
4444
55555
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
22
333
4444
55555
```
