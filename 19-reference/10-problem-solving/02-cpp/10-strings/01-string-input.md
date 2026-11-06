# String Input

Write a C++ program to take a string as input and display the entered string.

## Program

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string text;

    cout << "Enter a string: ";
    getline(cin, text);

    cout << "You entered: " << text << endl;

    return 0;
}
```

## Sample Output

```text
Enter a string: Hello C++ Programming
You entered: Hello C++ Programming
```
