# Octal to Hexadecimal

Write a C++ program to convert an octal number into its hexadecimal equivalent.

## Program

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string octal;
    long long decimal = 0;

    cout << "Enter an octal number: ";
    cin >> octal;

    for (char ch : octal) {
        if (ch < '0' || ch > '7') {
            cout << "Invalid octal number." << endl;
            return 0;
        }

        decimal = decimal * 8 + (ch - '0');
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
Enter an octal number: 377
Hexadecimal: FF
```
