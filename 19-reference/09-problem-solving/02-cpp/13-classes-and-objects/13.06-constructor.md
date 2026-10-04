# Constructor

Write a C++ program to demonstrate a constructor that initializes an object's data members.

## Program

```cpp
#include <iostream>
using namespace std;

class Student {
private:
    string name;
    int age;

public:
    Student(string studentName, int studentAge) {
        name = studentName;
        age = studentAge;
    }

    void display() {
        cout << "Name: " << name << endl;
        cout << "Age: " << age << endl;
    }
};

int main() {
    Student student("Rahim", 21);

    student.display();

    return 0;
}
```

## Sample Output

```text
Name: Rahim
Age: 21
```
