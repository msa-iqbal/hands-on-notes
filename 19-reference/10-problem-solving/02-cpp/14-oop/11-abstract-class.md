# Abstract Class

Write a C++ program to demonstrate an abstract class containing a pure virtual function.

## Program

```cpp
#include <iostream>
using namespace std;

class Shape {
public:
    virtual double area() = 0;

    virtual ~Shape() = default;
};

class Rectangle : public Shape {
private:
    double length;
    double width;

public:
    Rectangle(double l, double w) {
        length = l;
        width = w;
    }

    double area() override {
        return length * width;
    }
};

int main() {
    Rectangle rectangle(10, 5);

    cout << "Rectangle area: "
         << rectangle.area() << endl;

    return 0;
}
```

## Sample Output

```text
Rectangle area: 50
```
