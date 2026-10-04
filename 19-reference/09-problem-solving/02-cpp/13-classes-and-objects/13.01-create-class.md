# Create Class

Write a C++ program to create a simple class with a data member and a member function.

## Program

```cpp
#include <iostream>
using namespace std;

class Student {
public:
    string name;

    void display() {
        cout << "Student name: " << name << endl;
    }
};

int main() {
    Student student;

    student.name = "Rahim";
    student.display();

    return 0;
}
```

## Sample Output

```text
Student name: Rahim
```
