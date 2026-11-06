# Character Classification

Write a C++ program to classify an entered character using standard character classification functions.

The program checks whether the character is:

- Alphabetic
- Digit
- Alphanumeric
- Uppercase
- Lowercase
- Whitespace
- Punctuation

## Program

```cpp
#include <cctype>
#include <iostream>
using namespace std;

int main() {
    char ch;

    cout << "Enter a character: ";
    cin.get(ch);

    unsigned char value = static_cast<unsigned char>(ch);

    cout << boolalpha;
    cout << "Alphabetic: " << (isalpha(value) != 0) << endl;
    cout << "Digit: " << (isdigit(value) != 0) << endl;
    cout << "Alphanumeric: " << (isalnum(value) != 0) << endl;
    cout << "Uppercase: " << (isupper(value) != 0) << endl;
    cout << "Lowercase: " << (islower(value) != 0) << endl;
    cout << "Whitespace: " << (isspace(value) != 0) << endl;
    cout << "Punctuation: " << (ispunct(value) != 0) << endl;

    return 0;
}
```

## Sample Output

```text
Enter a character: A
Alphabetic: true
Digit: false
Alphanumeric: true
Uppercase: true
Lowercase: false
Whitespace: false
Punctuation: false
```
