# Alphabetic Left Angle Triangle 01

Write a C++ program to print a left-aligned right-angle triangle using the letter `A`.

## Pattern

```text
    A
   AA
  AAA
 AAAA
AAAAA
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
        for (int space = 1; space <= n - i; space++) {
            cout << " ";
        }

        for (int j = 1; j <= i; j++) {
            cout << 'A';
        }

        cout << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter number of rows: 5
    A
   AA
  AAA
 AAAA
AAAAA
```
