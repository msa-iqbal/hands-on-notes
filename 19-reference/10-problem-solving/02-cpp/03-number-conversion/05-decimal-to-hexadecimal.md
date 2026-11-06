# Decimal to Hexadecimal

Write a C++ program to convert a decimal number into its hexadecimal equivalent.

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
        cout << "Hexadecimal: 0" << endl;
        return 0;
    }

    char hexadecimal[32];
    int index = 0;

    while (decimal > 0) {
        int remainder = decimal % 16;

        if (remainder < 10) {
            hexadecimal[index++] = '0' + remainder;
        } else {
            hexadecimal[index++] = 'A' + (remainder - 10);
        }

        decimal /= 16;
    }

    cout << "Hexadecimal: ";

    for (int i = index - 1; i >= 0; i--) {
        cout << hexadecimal[i];
    }

    cout << endl;

    return 0;
}
```

## Sample Output

```text
Enter a decimal number: 255
Hexadecimal: FF
```
