# Capital or Small Letter

Write a C++ program to determine whether an entered alphabetic character is a capital letter or a small letter.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    char ch;

    cout << "Enter an alphabetic character: ";
    cin >> ch;

    if (ch >= 'A' && ch <= 'Z') {
        cout << ch << " is a capital letter." << endl;
    } else if (ch >= 'a' && ch <= 'z') {
        cout << ch << " is a small letter." << endl;
    } else {
        cout << ch << " is not an alphabetic character." << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter an alphabetic character: R
R is a capital letter.
```
