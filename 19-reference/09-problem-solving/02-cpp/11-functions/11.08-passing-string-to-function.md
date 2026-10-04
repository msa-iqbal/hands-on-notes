# Passing String to Function

Write a C++ program to pass a string to a function and display it.

## Program

```cpp
#include <iostream>
#include <string>
using namespace std;

void displayString(const string& text) {
    cout << "String: " << text << endl;
}

int main() {
    string text;

    cout << "Enter a string: ";
    getline(cin, text);

    displayString(text);

    return 0;
}
```

## Sample Output

```text
Enter a string: C++ Programming
String: C++ Programming
```
