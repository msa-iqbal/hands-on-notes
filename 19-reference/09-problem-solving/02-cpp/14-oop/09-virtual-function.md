# Virtual Function

Write a C++ program to demonstrate runtime polymorphism using a virtual function and a base-class pointer.

## Program

```cpp
#include <iostream>
using namespace std;

class Animal {
public:
    virtual void sound() {
        cout << "Animal makes a sound." << endl;
    }

    virtual ~Animal() = default;
};

class Dog : public Animal {
public:
    void sound() override {
        cout << "Dog barks." << endl;
    }
};

class Cat : public Animal {
public:
    void sound() override {
        cout << "Cat meows." << endl;
    }
};

int main() {
    Dog dog;
    Cat cat;

    Animal* animal;

    animal = &dog;
    animal->sound();

    animal = &cat;
    animal->sound();

    return 0;
}
```

## Sample Output

```text
Dog barks.
Cat meows.
```
