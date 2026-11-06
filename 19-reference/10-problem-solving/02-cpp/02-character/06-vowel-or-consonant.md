# Vowel or Consonant

Write a C++ program to determine whether an entered alphabetic character is a vowel or a consonant.

## Program

```cpp
#include <cctype>
#include <iostream>
using namespace std;

int main() {
    char ch;

    cout << "Enter an alphabetic character: ";
    cin >> ch;

    ch = static_cast<char>(
        tolower(static_cast<unsigned char>(ch))
    );

    if (ch < 'a' || ch > 'z') {
        cout << "Please enter an alphabetic character." << endl;
    } else if (ch == 'a' || ch == 'e' ||
               ch == 'i' || ch == 'o' ||
               ch == 'u') {
        cout << ch << " is a vowel." << endl;
    } else {
        cout << ch << " is a consonant." << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter an alphabetic character: E
e is a vowel.
```
