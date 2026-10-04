# Binary to Decimal

Write a C++ program to convert a binary number into its decimal equivalent.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    long long binary;
    int decimal = 0;
    int base = 1;

    cout << "Enter a binary number: ";
    cin >> binary;

    if (binary < 0) {
        cout << "Please enter a non-negative binary number." << endl;
        return 0;
    }

    if (binary == 0) {
        cout << "Decimal: 0" << endl;
        return 0;
    }

    while (binary > 0) {
        int digit = binary % 10;

        if (digit != 0 && digit != 1) {
            cout << "Invalid binary number." << endl;
            return 0;
        }

        decimal += digit * base;
        base *= 2;
        binary /= 10;
    }

    cout << "Decimal: " << decimal << endl;

    return 0;
}
```

## Sample Output

```text
Enter a binary number: 11001
Decimal: 25
```
