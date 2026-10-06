# Function Overriding

Write a C++ program to demonstrate function overriding, where a derived class provides its own implementation of a base class function.

## Program

```cpp
#include <iostream>
using namespace std;

class Animal {
public:
    void sound() {
        cout << "Animal makes a sound." << endl;
    }
};

class Dog : public Animal {
public:
    void sound() {
        cout << "Dog barks." << endl;
    }
};

int main() {
    Dog dog;

    dog.sound();

    return 0;
}
```

## Sample Output

```text
Dog barks.
```
