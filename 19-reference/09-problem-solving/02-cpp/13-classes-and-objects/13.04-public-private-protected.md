# Public, Private and Protected

Write a C++ program to demonstrate the `public`, `private`, and `protected` access specifiers.

## Program

```cpp
#include <iostream>
using namespace std;

class Parent {
public:
    int publicValue = 10;

protected:
    int protectedValue = 20;

private:
    int privateValue = 30;

public:
    void showPrivateValue() {
        cout << "Private value: " << privateValue << endl;
    }
};

class Child : public Parent {
public:
    void showProtectedValue() {
        cout << "Protected value: " << protectedValue << endl;
    }
};

int main() {
    Child child;

    cout << "Public value: " << child.publicValue << endl;

    child.showProtectedValue();
    child.showPrivateValue();

    return 0;
}
```

## Sample Output

```text
Public value: 10
Protected value: 20
Private value: 30
```
