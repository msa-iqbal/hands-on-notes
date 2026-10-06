# Hexadecimal to Octal

Write a C++ program to convert a hexadecimal number into its octal equivalent.

## Program

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string hexadecimal;
    long long decimal = 0;

    cout << "Enter a hexadecimal number: ";
    cin >> hexadecimal;

    for (char ch : hexadecimal) {
        int value;

        if (ch >= '0' && ch <= '9') {
            value = ch - '0';
        } else if (ch >= 'A' && ch <= 'F') {
            value = ch - 'A' + 10;
        } else if (ch >= 'a' && ch <= 'f') {
            value = ch - 'a' + 10;
        } else {
            cout << "Invalid hexadecimal number." << endl;
            return 0;
        }

        decimal = decimal * 16 + value;
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
Enter a hexadecimal number: FF
Octal: 377
```
