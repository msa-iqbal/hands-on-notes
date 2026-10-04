# String Tokenization

Write a C++ program to split a string into individual words.

## Program

```cpp
#include <iostream>
#include <string>
#include <sstream>
using namespace std;

int main() {
    string text;

    cout << "Enter a sentence: ";
    getline(cin, text);

    stringstream stream(text);
    string word;

    cout << "Tokens:" << endl;

    while (stream >> word) {
        cout << word << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter a sentence: C++ is a powerful programming language
Tokens:
C++
is
a
powerful
programming
language
```
