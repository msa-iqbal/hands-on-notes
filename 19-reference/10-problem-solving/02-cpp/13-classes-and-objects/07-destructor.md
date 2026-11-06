# Destructor

Write a C++ program to demonstrate a destructor that is automatically called when an object is destroyed.

## Program

```cpp
#include <iostream>
using namespace std;

class Demo {
public:
    Demo() {
        cout << "Constructor called." << endl;
    }

    ~Demo() {
        cout << "Destructor called." << endl;
    }
};

int main() {
    cout << "Creating object..." << endl;

    {
        Demo object;
        cout << "Object is inside the block." << endl;
    }

    cout << "Object has been destroyed." << endl;

    return 0;
}
```

## Sample Output

```text
Creating object...
Constructor called.
Object is inside the block.
Destructor called.
Object has been destroyed.
```
