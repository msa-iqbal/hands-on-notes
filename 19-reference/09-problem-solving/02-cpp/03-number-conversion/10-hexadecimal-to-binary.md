# Hexadecimal to Binary

Write a C++ program to convert a hexadecimal number into its binary equivalent.

## Program

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string hexadecimal;
    string binary;

    cout << "Enter a hexadecimal number: ";
    cin >> hexadecimal;

    for (char ch : hexadecimal) {
        switch (ch) {
            case '0': binary += "0000"; break;
            case '1': binary += "0001"; break;
            case '2': binary += "0010"; break;
            case '3': binary += "0011"; break;
            case '4': binary += "0100"; break;
            case '5': binary += "0101"; break;
            case '6': binary += "0110"; break;
            case '7': binary += "0111"; break;
            case '8': binary += "1000"; break;
            case '9': binary += "1001"; break;
            case 'A':
            case 'a': binary += "1010"; break;
            case 'B':
            case 'b': binary += "1011"; break;
            case 'C':
            case 'c': binary += "1100"; break;
            case 'D':
            case 'd': binary += "1101"; break;
            case 'E':
            case 'e': binary += "1110"; break;
            case 'F':
            case 'f': binary += "1111"; break;
            default:
                cout << "Invalid hexadecimal number." << endl;
                return 0;
        }
    }

    size_t firstOne = binary.find('1');

    if (firstOne == string::npos) {
        binary = "0";
    } else {
        binary = binary.substr(firstOne);
    }

    cout << "Binary: " << binary << endl;

    return 0;
}
```

## Sample Output

```text
Enter a hexadecimal number: 2F
Binary: 101111
```
