# this Pointer

Write a C++ program to use the `this` pointer to refer to the current object's data members.

## Program

```cpp
#include <iostream>
using namespace std;

class Student {
private:
    string name;
    int age;

public:
    void setData(string name, int age) {
        this->name = name;
        this->age = age;
    }

    void display() {
        cout << "Name: " << this->name << endl;
        cout << "Age: " << this->age << endl;
    }
};

int main() {
    Student student;

    student.setData("Karim", 22);
    student.display();

    return 0;
}
```

## Sample Output

```text
Name: Karim
Age: 22
```
