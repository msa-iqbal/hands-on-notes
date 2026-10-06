# Static Members

Write a C++ program to demonstrate a static data member shared by all objects of a class.

## Program

```cpp
#include <iostream>
using namespace std;

class Student {
private:
    static int count;

public:
    Student() {
        count++;
    }

    static void displayCount() {
        cout << "Number of objects: " << count << endl;
    }
};

int Student::count = 0;

int main() {
    Student student1;
    Student student2;
    Student student3;

    Student::displayCount();

    return 0;
}
```

## Sample Output

```text
Number of objects: 3
```
