# Variables

Write a C++ program to declare variables of different types, assign values to them, and display their values.

## C++ Program

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    int age = 25;
    double salary = 50000.50;
    char grade = 'A';
    bool isStudent = true;
    string name = "Alice";

    cout << "Name: " << name << endl;
    cout << "Age: " << age << endl;
    cout << "Salary: " << salary << endl;
    cout << "Grade: " << grade << endl;
    cout << "Student: " << boolalpha << isStudent << endl;

    return 0;
}
```

## Sample Output

```text
Name: Alice
Age: 25
Salary: 50000.5
Grade: A
Student: true
```
