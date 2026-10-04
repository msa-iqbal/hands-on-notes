# Multilevel Inheritance

Write a C++ program to demonstrate multilevel inheritance, where a class is derived from another derived class.

## Program

```cpp
#include <iostream>
using namespace std;

class Grandparent {
public:
    void showGrandparent() {
        cout << "Grandparent class." << endl;
    }
};

class Parent : public Grandparent {
public:
    void showParent() {
        cout << "Parent class." << endl;
    }
};

class Child : public Parent {
public:
    void showChild() {
        cout << "Child class." << endl;
    }
};

int main() {
    Child child;

    child.showGrandparent();
    child.showParent();
    child.showChild();

    return 0;
}
```

## Sample Output

```text
Grandparent class.
Parent class.
Child class.
```
