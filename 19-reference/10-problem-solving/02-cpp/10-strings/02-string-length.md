# String Length

Write a C++ program to find the length of a string.

## Program

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string text;

    cout << "Enter a string: ";
    getline(cin, text);

    cout << "String length = " << text.length() << endl;

    return 0;
}
```

## Sample Output

```text
Enter a string: Hello C++
String length = 9
```
