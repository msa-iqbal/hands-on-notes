# Pair

Write a C++ program to demonstrate the STL `pair` container.

## Program

```cpp
#include <iostream>
#include <utility>
#include <string>
using namespace std;

int main() {
    pair<string, int> student = {"Rahim", 85};

    cout << "Name: " << student.first << endl;
    cout << "Marks: " << student.second << endl;

    pair<string, int> anotherStudent;

    anotherStudent = make_pair("Karim", 90);

    cout << endl;
    cout << "Name: " << anotherStudent.first << endl;
    cout << "Marks: " << anotherStudent.second << endl;

    return 0;
}
```

## Sample Output

```text
Name: Rahim
Marks: 85

Name: Karim
Marks: 90
```
