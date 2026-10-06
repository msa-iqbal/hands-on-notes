# String Uppercase and Lowercase

Write a C++ program to convert a string to uppercase and lowercase.

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

    string uppercase = text;
    string lowercase = text;

    for (char& ch : uppercase) {
        ch = static_cast<char>(
            toupper(static_cast<unsigned char>(ch))
        );
    }

    for (char& ch : lowercase) {
        ch = static_cast<char>(
            tolower(static_cast<unsigned char>(ch))
        );
    }

    cout << "Uppercase: " << uppercase << endl;
    cout << "Lowercase: " << lowercase << endl;

    return 0;
}
```

## Sample Output

```text
Enter a string: Hello Cpp
Uppercase: HELLO CPP
Lowercase: hello cpp
```
