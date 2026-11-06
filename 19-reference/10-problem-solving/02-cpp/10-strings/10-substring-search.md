# Substring Search

Write a C++ program to search for a substring inside a string.

## Program

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string text;
    string substring;

    cout << "Enter main string: ";
    getline(cin, text);

    cout << "Enter substring to search: ";
    getline(cin, substring);

    size_t position = text.find(substring);

    if (position != string::npos) {
        cout << "Substring found at position: "
             << position + 1 << endl;
    } else {
        cout << "Substring not found." << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter main string: C++ Programming Language
Enter substring to search: Programming
Substring found at position: 5
```
