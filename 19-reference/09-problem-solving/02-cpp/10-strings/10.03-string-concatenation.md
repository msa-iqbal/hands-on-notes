# String Concatenation

Write a C++ program to concatenate two strings.

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

    string result = first + " " + second;

    cout << "Concatenated string: " << result << endl;

    return 0;
}
```

## Sample Output

```text
Enter first string: Hello
Enter second string: World
Concatenated string: Hello World
```
