# Create Object

Write a C++ program to create an object from a class and access its data members and member functions.

## Program

```cpp
#include <iostream>
using namespace std;

class Student {
public:
    string name;
    int age;

    void display() {
        cout << "Name: " << name << endl;
        cout << "Age: " << age << endl;
    }
};

int main() {
    Student student;

    student.name = "Karim";
    student.age = 20;

    student.display();

    return 0;
}
```

## Sample Output

```text
Name: Karim
Age: 20
```
