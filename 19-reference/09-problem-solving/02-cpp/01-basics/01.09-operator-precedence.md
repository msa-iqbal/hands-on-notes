# Operator Precedence

Write a C++ program to demonstrate how operator precedence and associativity affect the evaluation of expressions.

## C++ Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int result1 = 10 + 5 * 2;
    int result2 = (10 + 5) * 2;
    int result3 = 20 / 5 * 2;
    int result4 = 10 - 5 + 2;

    cout << "10 + 5 * 2 = " << result1 << endl;
    cout << "(10 + 5) * 2 = " << result2 << endl;
    cout << "20 / 5 * 2 = " << result3 << endl;
    cout << "10 - 5 + 2 = " << result4 << endl;

    return 0;
}
```

## Sample Output

```text
10 + 5 * 2 = 20
(10 + 5) * 2 = 30
20 / 5 * 2 = 8
10 - 5 + 2 = 7
```

> Multiplication and division have higher precedence than addition and subtraction. Parentheses can be used to explicitly control the order of evaluation.
