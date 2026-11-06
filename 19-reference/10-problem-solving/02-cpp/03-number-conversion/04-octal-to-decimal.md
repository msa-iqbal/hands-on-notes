# Octal to Decimal

Write a C++ program to convert an octal number into its decimal equivalent.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    long long octal;
    int decimal = 0;
    int base = 1;

    cout << "Enter an octal number: ";
    cin >> octal;

    if (octal < 0) {
        cout << "Please enter a non-negative octal number." << endl;
        return 0;
    }

    if (octal == 0) {
        cout << "Decimal: 0" << endl;
        return 0;
    }

    while (octal > 0) {
        int digit = octal % 10;

        if (digit < 0 || digit > 7) {
            cout << "Invalid octal number." << endl;
            return 0;
        }

        decimal += digit * base;
        base *= 8;
        octal /= 10;
    }

    cout << "Decimal: " << decimal << endl;

    return 0;
}
```

## Sample Output

```text
Enter an octal number: 123
Decimal: 83
```
