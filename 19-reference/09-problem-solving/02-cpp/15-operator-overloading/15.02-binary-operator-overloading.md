# Binary Operator Overloading

Write a C++ program to overload the binary `+` operator to add two objects.

## Program

```cpp
#include <iostream>
using namespace std;

class Number {
private:
    int value;

public:
    Number(int v = 0) {
        value = v;
    }

    Number operator+(const Number& other) const {
        return Number(value + other.value);
    }

    void display() const {
        cout << "Value: " << value << endl;
    }
};

int main() {
    Number first(20);
    Number second(30);

    Number result = first + second;

    cout << "First number: ";
    first.display();

    cout << "Second number: ";
    second.display();

    cout << "Sum: ";
    result.display();

    return 0;
}
```

## Sample Output

```text
First number: Value: 20
Second number: Value: 30
Sum: Value: 50
```
