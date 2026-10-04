# Class Members

Write a C++ program to demonstrate data members and member functions inside a class.

## Program

```cpp
#include <iostream>
using namespace std;

class Rectangle {
public:
    double length;
    double width;

    double area() {
        return length * width;
    }

    double perimeter() {
        return 2 * (length + width);
    }
};

int main() {
    Rectangle rectangle;

    rectangle.length = 10;
    rectangle.width = 5;

    cout << "Length: " << rectangle.length << endl;
    cout << "Width: " << rectangle.width << endl;
    cout << "Area: " << rectangle.area() << endl;
    cout << "Perimeter: " << rectangle.perimeter() << endl;

    return 0;
}
```

## Sample Output

```text
Length: 10
Width: 5
Area: 50
Perimeter: 30
```
