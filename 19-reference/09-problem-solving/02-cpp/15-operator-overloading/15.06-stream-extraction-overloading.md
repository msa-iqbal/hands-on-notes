# Stream Extraction Operator Overloading

Write a C++ program to overload the stream extraction operator `>>` to take object data using `cin`.

## Program

```cpp
#include <iostream>
using namespace std;

class Student {
private:
    string name;
    int age;

public:
    friend istream& operator>>(istream& input, Student& student) {
        input >> student.name >> student.age;
        return input;
    }

    friend ostream& operator<<(ostream& output, const Student& student) {
        output << "Name: " << student.name << endl;
        output << "Age: " << student.age;
        return output;
    }
};

int main() {
    Student student;

    cout << "Enter name and age: ";
    cin >> student;

    cout << student << endl;

    return 0;
}
```

## Sample Output

```text
Enter name and age: Karim 22
Name: Karim
Age: 22
```
