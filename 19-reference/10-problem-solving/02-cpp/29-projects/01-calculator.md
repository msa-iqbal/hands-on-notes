# Calculator

> A simple menu-driven C++ calculator project.

### Purpose

This project demonstrates how to build a console calculator using:

- Functions
- `switch`
- Loops
- User input
- Arithmetic operators
- Input validation

### Features

- Addition
- Subtraction
- Multiplication
- Division
- Division-by-zero protection
- Multiple calculations
- Exit option

### Basic Calculator

```cpp
#include <iostream>
using namespace std;

int main() {
    double a, b;
    char op;

    cout << "Enter first number: ";
    cin >> a;

    cout << "Enter operator (+, -, *, /): ";
    cin >> op;

    cout << "Enter second number: ";
    cin >> b;

    switch (op) {
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
                cout << "Error: Cannot divide by zero." << endl;
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

### Example Run

```text
Enter first number: 20
Enter operator (+, -, *, /): *
Enter second number: 5

Result = 100
```

### Function-Based Calculator

```cpp
#include <iostream>
using namespace std;

double add(double a, double b) {
    return a + b;
}

double subtract(double a, double b) {
    return a - b;
}

double multiply(double a, double b) {
    return a * b;
}

double divide(double a, double b) {
    return a / b;
}

int main() {
    double a, b;
    char op;

    cout << "Enter expression: ";
    cin >> a >> op >> b;

    switch (op) {
        case '+':
            cout << "Result = " << add(a, b) << endl;
            break;

        case '-':
            cout << "Result = " << subtract(a, b) << endl;
            break;

        case '*':
            cout << "Result = " << multiply(a, b) << endl;
            break;

        case '/':
            if (b == 0) {
                cout << "Error: Division by zero." << endl;
            } else {
                cout << "Result = " << divide(a, b) << endl;
            }
            break;

        default:
            cout << "Invalid operator." << endl;
    }

    return 0;
}
```

### Menu-Driven Calculator

```cpp
#include <iostream>
using namespace std;

double add(double a, double b) {
    return a + b;
}

double subtract(double a, double b) {
    return a - b;
}

double multiply(double a, double b) {
    return a * b;
}

double divide(double a, double b) {
    return a / b;
}

int main() {
    int choice;
    double a, b;

    do {
        cout << "\n===== Calculator =====\n";
        cout << "1. Addition\n";
        cout << "2. Subtraction\n";
        cout << "3. Multiplication\n";
        cout << "4. Division\n";
        cout << "5. Exit\n";
        cout << "Choose: ";
        cin >> choice;

        if (choice >= 1 && choice <= 4) {
            cout << "Enter first number: ";
            cin >> a;

            cout << "Enter second number: ";
            cin >> b;
        }

        switch (choice) {
            case 1:
                cout << "Result = " << add(a, b) << endl;
                break;

            case 2:
                cout << "Result = " << subtract(a, b) << endl;
                break;

            case 3:
                cout << "Result = " << multiply(a, b) << endl;
                break;

            case 4:
                if (b == 0) {
                    cout << "Error: Division by zero." << endl;
                } else {
                    cout << "Result = " << divide(a, b) << endl;
                }
                break;

            case 5:
                cout << "Calculator closed." << endl;
                break;

            default:
                cout << "Invalid choice." << endl;
        }

    } while (choice != 5);

    return 0;
}
```

### Example Output

```text
===== Calculator =====
1. Addition
2. Subtraction
3. Multiplication
4. Division
5. Exit
Choose: 1
Enter first number: 25
Enter second number: 15
Result = 40

===== Calculator =====
1. Addition
2. Subtraction
3. Multiplication
4. Division
5. Exit
Choose: 4
Enter first number: 10
Enter second number: 0
Error: Division by zero.
```

### Key Concepts

- Functions
- `switch`
- `do-while`
- Arithmetic operators
- Conditional statements
- Input validation

### Possible Improvements

- Scientific operations
- Modulus
- Power
- Square root
- Percentage
- Calculation history
- File-based history
- Class-based calculator
