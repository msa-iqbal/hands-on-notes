# Student Grade System

Write a C++ program to determine a student's grade from their marks.

The grading system used here is:

|Marks|Grade|
|--:|:-:|
|80–100|A+|
|70–79|A|
|60–69|A-|
|50–59|B|
|40–49|C|
|33–39|D|
|0–32|F|

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    double marks;

    cout << "Enter marks: ";
    cin >> marks;

    if (marks < 0 || marks > 100) {
        cout << "Invalid marks." << endl;
    } else if (marks >= 80) {
        cout << "Grade: A+" << endl;
    } else if (marks >= 70) {
        cout << "Grade: A" << endl;
    } else if (marks >= 60) {
        cout << "Grade: A-" << endl;
    } else if (marks >= 50) {
        cout << "Grade: B" << endl;
    } else if (marks >= 40) {
        cout << "Grade: C" << endl;
    } else if (marks >= 33) {
        cout << "Grade: D" << endl;
    } else {
        cout << "Grade: F" << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter marks: 85
Grade: A+
```
