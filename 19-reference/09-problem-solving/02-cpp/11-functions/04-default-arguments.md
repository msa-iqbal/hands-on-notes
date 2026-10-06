# Default Arguments

Write a C++ program to demonstrate default arguments in a function.

## Program

```cpp
#include <iostream>
using namespace std;

void display(string name = "Guest", int age = 18) {
    cout << "Name: " << name << endl;
    cout << "Age: " << age << endl;
}

int main() {
    display();

    cout << endl;

    display("Rahim", 25);

    return 0;
}
```

## Sample Output

```text
Name: Guest
Age: 18

Name: Rahim
Age: 25
```
