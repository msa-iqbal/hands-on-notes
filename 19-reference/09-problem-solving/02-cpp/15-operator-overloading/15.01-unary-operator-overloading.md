# Unary Operator Overloading

Write a C++ program to overload the unary `-` operator for a class.

## Program

```cpp
#include <iostream>
using namespace std;

class Number {
private:
    int value;

public:
    Number(int v) {
        value = v;
    }

    Number operator-() const {
        return Number(-value);
    }

    void display() const {
        cout << "Value: " << value << endl;
    }
};

int main() {
    Number number(25);

    cout << "Original number:" << endl;
    number.display();

    Number negative = -number;

    cout << "After unary minus:" << endl;
    negative.display();

    return 0;
}
```

## Sample Output

```text
Original number:
Value: 25
After unary minus:
Value: -25
```
