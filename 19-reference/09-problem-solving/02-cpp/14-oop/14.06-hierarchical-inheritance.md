# Hierarchical Inheritance

Write a C++ program to demonstrate hierarchical inheritance, where multiple derived classes inherit from the same base class.

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

class Cat : public Animal {
public:
    void meow() {
        cout << "Cat is meowing." << endl;
    }
};

int main() {
    Dog dog;
    Cat cat;

    cout << "Dog:" << endl;
    dog.eat();
    dog.bark();

    cout << endl;

    cout << "Cat:" << endl;
    cat.eat();
    cat.meow();

    return 0;
}
```

## Sample Output

```text
Dog:
Animal is eating.
Dog is barking.

Cat:
Animal is eating.
Cat is meowing.
```
