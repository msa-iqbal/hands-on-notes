# Count Vowels, Consonants, Digits and Words

Write a C++ program to count vowels, consonants, digits, and words in a string.

## Program

```cpp
#include <iostream>
#include <string>
#include <cctype>
#include <sstream>
using namespace std;

int main() {
    string text;

    cout << "Enter a string: ";
    getline(cin, text);

    int vowels = 0;
    int consonants = 0;
    int digits = 0;
    int words = 0;

    for (char ch : text) {
        if (isalpha(static_cast<unsigned char>(ch))) {
            char lower = static_cast<char>(
                tolower(static_cast<unsigned char>(ch))
            );

            if (lower == 'a' ||
                lower == 'e' ||
                lower == 'i' ||
                lower == 'o' ||
                lower == 'u') {
                vowels++;
            } else {
                consonants++;
            }
        } else if (isdigit(static_cast<unsigned char>(ch))) {
            digits++;
        }
    }

    stringstream stream(text);
    string word;

    while (stream >> word) {
        words++;
    }

    cout << "Vowels = " << vowels << endl;
    cout << "Consonants = " << consonants << endl;
    cout << "Digits = " << digits << endl;
    cout << "Words = " << words << endl;

    return 0;
}
```

## Sample Output

```text
Enter a string: Hello World 123
Vowels = 3
Consonants = 7
Digits = 3
Words = 3
```
