# Input and Output

Write a C++ program to take input from the user and display the entered values.

## C++ Program

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string name;
    int age;

    cout << "Enter your name: ";
    cin >> name;

    cout << "Enter your age: ";
    cin >> age;

    cout << "\nName: " << name << endl;
    cout << "Age: " << age << endl;

    return 0;
}
```

## Sample Output

```text
Enter your name: Alice
Enter your age: 25

Name: Alice
Age: 25
```
