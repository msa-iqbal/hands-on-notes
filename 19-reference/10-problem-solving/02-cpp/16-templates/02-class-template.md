# Class Template

Write a C++ program to create a class template that can store and display values of different data types.

## Program

```cpp
#include <iostream>
#include <string>
using namespace std;

template <typename T>
class Box {
private:
    T value;

public:
    Box(T v) {
        value = v;
    }

    void display() const {
        cout << "Value: " << value << endl;
    }
};

int main() {
    Box<int> integerBox(100);
    Box<double> doubleBox(25.5);
    Box<string> stringBox("Hello C++");

    integerBox.display();
    doubleBox.display();
    stringBox.display();

    return 0;
}
```

## Sample Output

```text
Value: 100
Value: 25.5
Value: Hello C++
```
