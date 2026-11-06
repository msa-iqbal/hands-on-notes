# Reference Parameters

Write a C++ program to use reference parameters to swap two numbers.

## Program

```cpp
#include <iostream>
using namespace std;

void swapNumbers(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;
}

int main() {
    int a, b;

    cout << "Enter two numbers: ";
    cin >> a >> b;

    cout << "Before swap: " << a << " " << b << endl;

    swapNumbers(a, b);

    cout << "After swap: " << a << " " << b << endl;

    return 0;
}
```

## Sample Output

```text
Enter two numbers: 10 20
Before swap: 10 20
After swap: 20 10
```
