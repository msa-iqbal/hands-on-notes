# Find ASCII Value

Write a C++ program to input a character and find its ASCII value.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    char ch;

    cout << "Enter a character: ";
    cin >> ch;

    cout << "ASCII value of '" << ch << "' = "
         << static_cast<int>(ch) << endl;

    return 0;
}
```

## Sample Output

```text
Enter a character: A
ASCII value of 'A' = 65
```
