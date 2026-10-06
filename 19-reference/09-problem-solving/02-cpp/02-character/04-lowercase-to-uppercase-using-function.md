# Lowercase to Uppercase Using Function

Write a C++ program to convert a lowercase character into an uppercase character using the `toupper()` function.

## Program

```cpp
#include <cctype>
#include <iostream>
using namespace std;

int main() {
    char ch;

    cout << "Enter a lowercase character: ";
    cin >> ch;

    if (ch >= 'a' && ch <= 'z') {
        ch = static_cast<char>(
            toupper(static_cast<unsigned char>(ch))
        );

        cout << "Uppercase character: " << ch << endl;
    } else {
        cout << "Please enter a lowercase letter." << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter a lowercase character: m
Uppercase character: M
```
