# Alphabetic Right Angle Triangle 06

Write a C++ program to print a right-angle triangle using alternating uppercase and lowercase alphabets.

## Pattern

```text
A
aB
aBa
AbAb
AbAbA
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
            if ((i + j) % 2 == 0) {
                cout << char('A' + (j - 1));
            } else {
                cout << char('a' + (j - 1));
            }
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
aB
ABa
aBaB
ABaBa
```
