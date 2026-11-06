# Data Types

Write a C++ program to demonstrate common built-in data types and display their sizes.

## C++ Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int integerValue = 10;
    float floatValue = 10.5f;
    double doubleValue = 20.75;
    char characterValue = 'A';
    bool booleanValue = true;

    cout << "int value: " << integerValue << endl;
    cout << "float value: " << floatValue << endl;
    cout << "double value: " << doubleValue << endl;
    cout << "char value: " << characterValue << endl;
    cout << "bool value: " << boolalpha << booleanValue << endl;

    cout << "\nSizes of data types:" << endl;
    cout << "int: " << sizeof(int) << " bytes" << endl;
    cout << "float: " << sizeof(float) << " bytes" << endl;
    cout << "double: " << sizeof(double) << " bytes" << endl;
    cout << "char: " << sizeof(char) << " byte" << endl;
    cout << "bool: " << sizeof(bool) << " byte" << endl;

    return 0;
}
```

## Sample Output

```text
int value: 10
float value: 10.5
double value: 20.75
char value: A
bool value: true

Sizes of data types:
int: 4 bytes
float: 4 bytes
double: 8 bytes
char: 1 byte
bool: 1 byte
```

> Note: The exact size of some C++ data types can vary depending on the compiler and platform.
