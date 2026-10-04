# Function Call Operator

Write a C++ program to overload the function call operator `()` so that an object can be used like a function.

## Program

```cpp
#include <iostream>
using namespace std;

class Calculator {
public:
    int operator()(int a, int b) const {
        return a + b;
    }
};

int main() {
    Calculator calculate;

    int result = calculate(20, 30);

    cout << "Result: " << result << endl;

    return 0;
}
```

## Sample Output

```text
Result: 50
```
