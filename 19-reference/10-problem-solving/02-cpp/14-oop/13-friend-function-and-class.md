# Friend Function and Class

Write a C++ program to demonstrate a friend function and a friend class accessing private members of another class.

## Program

```cpp
#include <iostream>
using namespace std;

class Student {
private:
    int marks;

public:
    Student(int value) {
        marks = value;
    }

    friend void showMarks(const Student& student);
    friend class Teacher;
};

void showMarks(const Student& student) {
    cout << "Marks using friend function: "
         << student.marks << endl;
}

class Teacher {
public:
    void displayMarks(const Student& student) {
        cout << "Marks using friend class: "
             << student.marks << endl;
    }
};

int main() {
    Student student(85);

    showMarks(student);

    Teacher teacher;
    teacher.displayMarks(student);

    return 0;
}
```

## Sample Output

```text
Marks using friend function: 85
Marks using friend class: 85
```
