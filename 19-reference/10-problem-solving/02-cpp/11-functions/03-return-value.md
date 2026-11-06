# Return Value

Write a C++ program to create a function that returns a value.

## Program

```cpp
#include <iostream>
using namespace std;

int add(int a, int b) {
    return a + b;
}

int main() {
    int a, b;

    cout << "Enter two numbers: ";
    cin >> a >> b;

    int result = add(a, b);

    cout << "Sum = " << result << endl;

    return 0;
}
```

## Sample Output

```text
Enter two numbers: 25 35
Sum = 60
```
