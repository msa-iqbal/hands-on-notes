# Stream Insertion Operator Overloading

Write a C++ program to overload the stream insertion operator `<<` to display an object using `cout`.

## Program

```cpp
#include <iostream>
using namespace std;

class Student {
private:
    string name;
    int age;

public:
    Student(string n, int a) {
        name = n;
        age = a;
    }

    friend ostream& operator<<(ostream& output, const Student& student) {
        output << "Name: " << student.name
               << ", Age: " << student.age;

        return output;
    }
};

int main() {
    Student student("Rahim", 21);

    cout << student << endl;

    return 0;
}
```

## Sample Output

```text
Name: Rahim, Age: 21
```
