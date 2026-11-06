# Comparison Operator Overloading

Write a C++ program to overload comparison operators to compare two objects.

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

    bool operator==(const Number& other) const {
        return value == other.value;
    }

    bool operator!=(const Number& other) const {
        return value != other.value;
    }

    bool operator<(const Number& other) const {
        return value < other.value;
    }

    bool operator>(const Number& other) const {
        return value > other.value;
    }

    int getValue() const {
        return value;
    }
};

int main() {
    Number first(20);
    Number second(30);

    cout << "First number: " << first.getValue() << endl;
    cout << "Second number: " << second.getValue() << endl;

    cout << boolalpha;

    cout << "Equal: " << (first == second) << endl;
    cout << "Not equal: " << (first != second) << endl;
    cout << "First is less: " << (first < second) << endl;
    cout << "First is greater: " << (first > second) << endl;

    return 0;
}
```

## Sample Output

```text
First number: 20
Second number: 30
Equal: false
Not equal: true
First is less: true
First is greater: false
```
