# Member Functions

Write a C++ program to define and use member functions inside and outside a class.

## Program

```cpp
#include <iostream>
using namespace std;

class Calculator {
public:
    int add(int a, int b);
    int subtract(int a, int b) {
        return a - b;
    }
};

int Calculator::add(int a, int b) {
    return a + b;
}

int main() {
    Calculator calculator;

    cout << "Addition: "
         << calculator.add(20, 10) << endl;

    cout << "Subtraction: "
         << calculator.subtract(20, 10) << endl;

    return 0;
}
```

## Sample Output

```text
Addition: 30
Subtraction: 10
```
