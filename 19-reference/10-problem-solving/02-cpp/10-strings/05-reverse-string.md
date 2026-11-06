# Reverse String

Write a C++ program to reverse a string.

## Program

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string text;

    cout << "Enter a string: ";
    getline(cin, text);

    cout << "Reversed string: ";

    for (int i = static_cast<int>(text.length()) - 1; i >= 0; i--) {
        cout << text[i];
    }

    cout << endl;

    return 0;
}
```

## Sample Output

```text
Enter a string: Hello
Reversed string: olleH
```
