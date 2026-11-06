# Octal to Binary

Write a C++ program to convert an octal number into its binary equivalent.

## Program

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string octal;
    string binary;

    cout << "Enter an octal number: ";
    cin >> octal;

    for (char ch : octal) {
        if (ch < '0' || ch > '7') {
            cout << "Invalid octal number." << endl;
            return 0;
        }

        switch (ch) {
            case '0': binary += "000"; break;
            case '1': binary += "001"; break;
            case '2': binary += "010"; break;
            case '3': binary += "011"; break;
            case '4': binary += "100"; break;
            case '5': binary += "101"; break;
            case '6': binary += "110"; break;
            case '7': binary += "111"; break;
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
Enter an octal number: 55
Binary: 101101
```
