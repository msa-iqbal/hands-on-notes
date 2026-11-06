# Type Casting

Write a C++ program to demonstrate implicit and explicit type casting.

## C++ Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int integerValue = 10;
    double decimalValue = 5.75;

    // Implicit conversion: int to double
    double implicitResult = integerValue;

    // Explicit conversion using static_cast
    int explicitResult = static_cast<int>(decimalValue);

    cout << "Integer value: " << integerValue << endl;
    cout << "Decimal value: " << decimalValue << endl;

    cout << "Implicit conversion: " << implicitResult << endl;
    cout << "Explicit conversion: " << explicitResult << endl;

    return 0;
}
```

## Sample Output

```text
Integer value: 10
Decimal value: 5.75
Implicit conversion: 10
Explicit conversion: 5
```
