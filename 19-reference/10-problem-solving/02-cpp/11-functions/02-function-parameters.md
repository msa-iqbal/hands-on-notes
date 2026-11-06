# Function Parameters

Write a C++ program to pass parameters to a function and display their sum.

## Program

```cpp
#include <iostream>
using namespace std;

void add(int a, int b) {
    cout << "Sum = " << a + b << endl;
}

int main() {
    int a, b;

    cout << "Enter two numbers: ";
    cin >> a >> b;

    add(a, b);

    return 0;
}
```

## Sample Output

```text
Enter two numbers: 10 20
Sum = 30
```
