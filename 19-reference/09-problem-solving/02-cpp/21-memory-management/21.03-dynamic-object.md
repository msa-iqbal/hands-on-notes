# Dynamic Object

Objects can be dynamically allocated using `new`.

The object is destroyed using `delete`.

## Example

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
    Student* student = new Student;

    student->name = "Muhammad";
    student->age = 25;

    student->display();

    delete student;
    student = nullptr;

    return 0;
}
```

## Expected Output

```text
Name: Muhammad
Age: 25
```

## Constructor with Dynamic Object

```cpp
#include <iostream>

using namespace std;

class Student {
public:
    string name;
    int age;

    Student(string studentName, int studentAge)
        : name(studentName), age(studentAge) {
    }

    void display() {
        cout << name << " " << age << endl;
    }
};

int main() {
    Student* student =
        new Student("Muhammad", 25);

    student->display();

    delete student;

    return 0;
}
```

## Accessing Members

For an ordinary object:

```cpp
Student student;

student.display();
```

For a pointer to an object:

```cpp
Student* student = new Student;

student->display();
```

The `->` operator is used to access members through an object pointer.
