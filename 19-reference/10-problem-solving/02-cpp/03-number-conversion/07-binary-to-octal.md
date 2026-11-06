# Binary to Octal

Write a C++ program to convert a binary number into its octal equivalent.

## Program

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string binary;
    int decimal = 0;

    cout << "Enter a binary number: ";
    cin >> binary;

    for (char ch : binary) {
        if (ch != '0' && ch != '1') {
            cout << "Invalid binary number." << endl;
            return 0;
        }

        decimal = decimal * 2 + (ch - '0');
    }

    if (decimal == 0) {
        cout << "Octal: 0" << endl;
        return 0;
    }

    int octal[32];
    int index = 0;

    while (decimal > 0) {
        octal[index++] = decimal % 8;
        decimal /= 8;
    }

    cout << "Octal: ";

    for (int i = index - 1; i >= 0; i--) {
        cout << octal[i];
    }

    cout << endl;

    return 0;
}
```

## Sample Output

```text
Enter a binary number: 101101
Octal: 55
```
