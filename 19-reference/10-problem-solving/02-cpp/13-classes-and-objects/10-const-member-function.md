# Const Member Function

Write a C++ program to demonstrate a `const` member function that does not modify the object's data members.

## Program

```cpp
#include <iostream>
using namespace std;

class Rectangle {
private:
    double length;
    double width;

public:
    Rectangle(double l, double w) {
        length = l;
        width = w;
    }

    double area() const {
        return length * width;
    }

    void display() const {
        cout << "Length: " << length << endl;
        cout << "Width: " << width << endl;
        cout << "Area: " << area() << endl;
    }
};

int main() {
    const Rectangle rectangle(10, 5);

    rectangle.display();

    return 0;
}
```

## Sample Output

```text
Length: 10
Width: 5
Area: 50
```
