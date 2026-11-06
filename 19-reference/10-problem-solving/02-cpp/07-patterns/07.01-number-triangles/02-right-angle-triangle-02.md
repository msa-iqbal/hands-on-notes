# Right Angle Triangle 02

Write a C++ program to print a right-angle triangle using increasing numbers.

## Pattern

```text
1
12
123
1234
12345
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
12
123
1234
12345
```
