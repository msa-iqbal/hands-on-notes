# Hybrid Inheritance

Write a C++ program to demonstrate hybrid inheritance, which combines multiple types of inheritance.

## Program

```cpp
#include <iostream>
using namespace std;

class Person {
public:
    void showPerson() {
        cout << "Person class." << endl;
    }
};

class Student : virtual public Person {
public:
    void showStudent() {
        cout << "Student class." << endl;
    }
};

class Employee : virtual public Person {
public:
    void showEmployee() {
        cout << "Employee class." << endl;
    }
};

class TeachingAssistant : public Student, public Employee {
public:
    void showTeachingAssistant() {
        cout << "Teaching Assistant class." << endl;
    }
};

int main() {
    TeachingAssistant assistant;

    assistant.showPerson();
    assistant.showStudent();
    assistant.showEmployee();
    assistant.showTeachingAssistant();

    return 0;
}
```

## Sample Output

```text
Person class.
Student class.
Employee class.
Teaching Assistant class.
```
