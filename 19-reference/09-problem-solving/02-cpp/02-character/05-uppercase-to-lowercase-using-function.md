# Uppercase to Lowercase Using Function

Write a C++ program to convert an uppercase character into a lowercase character using the `tolower()` function.

## Program

```cpp
#include <cctype>
#include <iostream>
using namespace std;

int main() {
    char ch;

    cout << "Enter an uppercase character: ";
    cin >> ch;

    if (ch >= 'A' && ch <= 'Z') {
        ch = static_cast<char>(
            tolower(static_cast<unsigned char>(ch))
        );

        cout << "Lowercase character: " << ch << endl;
    } else {
        cout << "Please enter an uppercase letter." << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter an uppercase character: M
Lowercase character: m
```
