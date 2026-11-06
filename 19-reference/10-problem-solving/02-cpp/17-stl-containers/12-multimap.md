# Multimap

Write a C++ program to demonstrate an STL `multimap` that allows multiple values for the same key.

## Program

```cpp
#include <iostream>
#include <map>
using namespace std;

int main() {
    multimap<string, int> students;

    students.insert({"CSE", 85});
    students.insert({"CSE", 90});
    students.insert({"EEE", 80});
    students.insert({"CSE", 75});

    cout << "Department marks:" << endl;

    for (const auto& student : students) {
        cout << student.first << ": "
             << student.second << endl;
    }

    return 0;
}
```

## Sample Output

```text
Department marks:
CSE: 85
CSE: 90
CSE: 75
EEE: 80
```
