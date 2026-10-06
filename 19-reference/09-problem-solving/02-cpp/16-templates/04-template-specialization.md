# Template Specialization

Write a C++ program to demonstrate template specialization by providing a custom implementation for a specific data type.

## Program

```cpp
#include <iostream>
#include <string>
using namespace std;

template <typename T>
class Printer {
public:
    void print(T value) {
        cout << "Generic value: " << value << endl;
    }
};

template <>
class Printer<string> {
public:
    void print(string value) {
        cout << "String value: [" << value << "]" << endl;
    }
};

int main() {
    Printer<int> integerPrinter;
    Printer<string> stringPrinter;

    integerPrinter.print(100);
    stringPrinter.print("Hello C++");

    return 0;
}
```

## Sample Output

```text
Generic value: 100
String value: [Hello C++]
```
