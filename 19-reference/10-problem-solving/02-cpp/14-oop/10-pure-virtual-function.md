# Pure Virtual Function

Write a C++ program to demonstrate a pure virtual function using the `= 0` syntax.

## Program

```cpp
#include <iostream>
using namespace std;

class Shape {
public:
    virtual void draw() = 0;
};

class Circle : public Shape {
public:
    void draw() override {
        cout << "Drawing a circle." << endl;
    }
};

int main() {
    Circle circle;

    circle.draw();

    return 0;
}
```

## Sample Output

```text
Drawing a circle.
```
