# Template Default Arguments

Write a C++ program to demonstrate a template with a default template argument.

## Program

```cpp
#include <iostream>
using namespace std;

template <typename T = int>
class Number {
private:
    T value;

public:
    Number(T v) {
        value = v;
    }

    void display() const {
        cout << "Value: " << value << endl;
    }
};

int main() {
    Number<> integerNumber(100);
    Number<double> doubleNumber(25.5);

    integerNumber.display();
    doubleNumber.display();

    return 0;
}
```

## Sample Output

```text
Value: 100
Value: 25.5
```
