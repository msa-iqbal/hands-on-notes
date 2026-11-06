# Polymorphism

Write a C++ program to demonstrate runtime polymorphism using a common base-class interface and different derived-class implementations.

## Program

```cpp
#include <iostream>
using namespace std;

class Animal {
public:
    virtual void sound() const {
        cout << "Animal makes a sound." << endl;
    }

    virtual ~Animal() = default;
};

class Dog : public Animal {
public:
    void sound() const override {
        cout << "Dog barks." << endl;
    }
};

class Cat : public Animal {
public:
    void sound() const override {
        cout << "Cat meows." << endl;
    }
};

int main() {
    Dog dog;
    Cat cat;

    Animal* animals[] = {&dog, &cat};

    for (Animal* animal : animals) {
        animal->sound();
    }

    return 0;
}
```

## Sample Output

```text
Dog barks.
Cat meows.
```
