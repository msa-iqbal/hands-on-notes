# Left Angle Triangle 05

Write a C++ program to print a left-aligned right-angle triangle using consecutive numbers.

## Pattern

```text
    1
   23
  456
 78910
1112131415
```

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;
    int number = 1;

    cout << "Enter number of rows: ";
    cin >> n;

    for (int i = 1; i <= n; i++) {
        for (int space = 1; space <= n - i; space++) {
            cout << " ";
        }

        for (int j = 1; j <= i; j++) {
            cout << number;
            number++;
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
   23
  456
 78910
1112131415
```
