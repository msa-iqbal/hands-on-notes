# String Comparison

Write a C++ program to compare two strings.

## Program

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string first, second;

    cout << "Enter first string: ";
    getline(cin, first);

    cout << "Enter second string: ";
    getline(cin, second);

    if (first == second) {
        cout << "Strings are equal." << endl;
    } else if (first < second) {
        cout << "First string comes before second string." << endl;
    } else {
        cout << "First string comes after second string." << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter first string: apple
Enter second string: apple
Strings are equal.
```
