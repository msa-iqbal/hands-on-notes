# Unordered Map

Write a C++ program to demonstrate an STL `unordered_map`.

## Program

```cpp
#include <iostream>
#include <unordered_map>
using namespace std;

int main() {
    unordered_map<string, int> students;

    students["Rahim"] = 85;
    students["Karim"] = 90;
    students["Hasan"] = 78;

    cout << "Student marks:" << endl;

    for (const auto& student : students) {
        cout << student.first << ": "
             << student.second << endl;
    }

    return 0;
}
```

## Sample Output

```text
Student marks:
Hasan: 78
Karim: 90
Rahim: 85
```

> The iteration order of an `unordered_map` is not guaranteed.
