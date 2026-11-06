# Alphabetic Right Angle Triangle 03

Write a C++ program to print a right-angle triangle where each row contains the same alphabetic character.

## Pattern

```text
A
BB
CCC
DDDD
EEEEE
```

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;

    cout << "Enter number of rows: ";
    cin >> n;

    for (int i = 0; i < n; i++) {
        char ch = 'A' + i;

        for (int j = 0; j <= i; j++) {
            cout << ch;
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
BB
CCC
DDDD
EEEEE
```
