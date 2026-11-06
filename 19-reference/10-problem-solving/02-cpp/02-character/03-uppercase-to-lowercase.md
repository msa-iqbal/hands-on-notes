# Uppercase to Lowercase

Write a C++ program to convert an uppercase character into a lowercase character without using a library function.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    char ch;

    cout << "Enter an uppercase character: ";
    cin >> ch;

    if (ch >= 'A' && ch <= 'Z') {
        ch = ch - 'A' + 'a';
        cout << "Lowercase character: " << ch << endl;
    } else {
        cout << "Please enter an uppercase letter." << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter an uppercase character: G
Lowercase character: g
```
