# Decimal to Binary

Write a C++ program to convert a decimal number into its binary equivalent.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int decimal;

    cout << "Enter a decimal number: ";
    cin >> decimal;

    if (decimal == 0) {
        cout << "Binary: 0" << endl;
        return 0;
    }

    if (decimal < 0) {
        cout << "Please enter a non-negative number." << endl;
        return 0;
    }

    int binary[32];
    int index = 0;

    while (decimal > 0) {
        binary[index++] = decimal % 2;
        decimal /= 2;
    }

    cout << "Binary: ";

    for (int i = index - 1; i >= 0; i--) {
        cout << binary[i];
    }

    cout << endl;

    return 0;
}
```

## Sample Output

```text
Enter a decimal number: 25
Binary: 11001
```
