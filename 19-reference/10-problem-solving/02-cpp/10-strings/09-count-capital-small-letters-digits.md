# Count Capital Letters, Small Letters and Digits

Write a C++ program to count uppercase letters, lowercase letters, and digits in a string.

## Program

```cpp
#include <iostream>
#include <string>
#include <cctype>
using namespace std;

int main() {
    string text;

    cout << "Enter a string: ";
    getline(cin, text);

    int uppercase = 0;
    int lowercase = 0;
    int digits = 0;

    for (char ch : text) {
        if (isupper(static_cast<unsigned char>(ch))) {
            uppercase++;
        } else if (islower(static_cast<unsigned char>(ch))) {
            lowercase++;
        } else if (isdigit(static_cast<unsigned char>(ch))) {
            digits++;
        }
    }

    cout << "Capital letters = " << uppercase << endl;
    cout << "Small letters = " << lowercase << endl;
    cout << "Digits = " << digits << endl;

    return 0;
}
```

## Sample Output

```text
Enter a string: Hello C++ 123
Capital letters = 2
Small letters = 6
Digits = 3
```
