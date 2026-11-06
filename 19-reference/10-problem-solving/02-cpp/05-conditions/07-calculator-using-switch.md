# Calculator Using Switch

Write a C++ program to perform addition, subtraction, multiplication, and division using a `switch` statement.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    double a, b;
    char operation;

    cout << "Enter first number: ";
    cin >> a;

    cout << "Enter operator (+, -, *, /): ";
    cin >> operation;

    cout << "Enter second number: ";
    cin >> b;

    switch (operation) {
        case '+':
            cout << "Result = " << a + b << endl;
            break;

        case '-':
            cout << "Result = " << a - b << endl;
            break;

        case '*':
            cout << "Result = " << a * b << endl;
            break;

        case '/':
            if (b == 0) {
                cout << "Error: division by zero." << endl;
            } else {
                cout << "Result = " << a / b << endl;
            }
            break;

        default:
            cout << "Invalid operator." << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter first number: 20
Enter operator (+, -, *, /): *
Enter second number: 5
Result = 100
```
