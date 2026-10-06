# Multiple Inheritance

Write a C++ program to demonstrate multiple inheritance, where one derived class inherits from more than one base class.

## Program

```cpp
#include <iostream>
using namespace std;

class Father {
public:
    void showFather() {
        cout << "Father's property." << endl;
    }
};

class Mother {
public:
    void showMother() {
        cout << "Mother's property." << endl;
    }
};

class Child : public Father, public Mother {
public:
    void showChild() {
        cout << "Child's property." << endl;
    }
};

int main() {
    Child child;

    child.showFather();
    child.showMother();
    child.showChild();

    return 0;
}
```

## Sample Output

```text
Father's property.
Mother's property.
Child's property.
```
