# Single Inheritance

Write a C++ program to demonstrate single inheritance, where one derived class inherits from one base class.

## Program

```cpp
#include <iostream>
using namespace std;

class Person {
public:
    string name;

    void displayName() {
        cout << "Name: " << name << endl;
    }
};

class Student : public Person {
public:
    int rollNumber;

    void displayRollNumber() {
        cout << "Roll number: " << rollNumber << endl;
    }
};

int main() {
    Student student;

    student.name = "Rahim";
    student.rollNumber = 101;

    student.displayName();
    student.displayRollNumber();

    return 0;
}
```

## Sample Output

```text
Name: Rahim
Roll number: 101
```
