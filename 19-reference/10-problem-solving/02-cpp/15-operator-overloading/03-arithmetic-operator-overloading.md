# Arithmetic Operator Overloading

Write a C++ program to overload arithmetic operators such as `+`, `-`, `*`, and `/` for a class.

## Program

```cpp
#include <iostream>
using namespace std;

class Number {
private:
    double value;

public:
    Number(double v = 0) {
        value = v;
    }

    Number operator+(const Number& other) const {
        return Number(value + other.value);
    }

    Number operator-(const Number& other) const {
        return Number(value - other.value);
    }

    Number operator*(const Number& other) const {
        return Number(value * other.value);
    }

    Number operator/(const Number& other) const {
        return Number(value / other.value);
    }

    void display() const {
        cout << value << endl;
    }
};

int main() {
    Number first(20);
    Number second(5);

    cout << "Addition: ";
    (first + second).display();

    cout << "Subtraction: ";
    (first - second).display();

    cout << "Multiplication: ";
    (first * second).display();

    cout << "Division: ";
    (first / second).display();

    return 0;
}
```

## Sample Output

```text
Addition: 25
Subtraction: 15
Multiplication: 100
Division: 4
```
