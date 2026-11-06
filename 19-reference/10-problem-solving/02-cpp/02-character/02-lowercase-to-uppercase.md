# Lowercase to Uppercase

Write a C++ program to convert a lowercase character into an uppercase character without using a library function.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    char ch;

    cout << "Enter a lowercase character: ";
    cin >> ch;

    if (ch >= 'a' && ch <= 'z') {
        ch = ch - 'a' + 'A';
        cout << "Uppercase character: " << ch << endl;
    } else {
        cout << "Please enter a lowercase letter." << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter a lowercase character: g
Uppercase character: G
```
