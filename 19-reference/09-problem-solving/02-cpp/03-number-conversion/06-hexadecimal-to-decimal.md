# Hexadecimal to Decimal

Write a C++ program to convert a hexadecimal number into its decimal equivalent.

## Program

```cpp
#include <cctype>
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

    cout << "Decimal: " << decimal << endl;

    return 0;
}
```

## Sample Output

```text
Enter a hexadecimal number: FF
Decimal: 255
```
