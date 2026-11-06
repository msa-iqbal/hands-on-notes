# Decimal to Octal

Write a C++ program to convert a decimal number into its octal equivalent.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int decimal;

    cout << "Enter a decimal number: ";
    cin >> decimal;

    if (decimal < 0) {
        cout << "Please enter a non-negative number." << endl;
        return 0;
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
Enter a decimal number: 83
Octal: 123
```
