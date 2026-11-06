# Function Overloading

Write a C++ program to demonstrate function overloading using functions with different parameter types.

## Program

```cpp
#include <iostream>
using namespace std;

int add(int a, int b) {
    return a + b;
}

double add(double a, double b) {
    return a + b;
}

int add(int a, int b, int c) {
    return a + b + c;
}

int main() {
    cout << "Integer addition: " << add(10, 20) << endl;
    cout << "Double addition: " << add(10.5, 20.5) << endl;
    cout << "Three-number addition: " << add(10, 20, 30) << endl;

    return 0;
}
```

## Sample Output

```text
Integer addition: 30
Double addition: 31
Three-number addition: 60
```
