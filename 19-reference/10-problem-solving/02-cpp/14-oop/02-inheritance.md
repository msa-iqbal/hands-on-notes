# Inheritance

Write a C++ program to demonstrate inheritance where a derived class inherits members from a base class.

## Program

```cpp
#include <iostream>
using namespace std;

class Animal {
public:
    void eat() {
        cout << "Animal is eating." << endl;
    }
};

class Dog : public Animal {
public:
    void bark() {
        cout << "Dog is barking." << endl;
    }
};

int main() {
    Dog dog;

    dog.eat();
    dog.bark();

    return 0;
}
```

## Sample Output

```text
Animal is eating.
Dog is barking.
```
