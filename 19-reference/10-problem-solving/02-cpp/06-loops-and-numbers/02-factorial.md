# Factorial

Write a C++ program to find the factorial of a non-negative integer.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int number;
    unsigned long long factorial = 1;

    cout << "Enter a non-negative integer: ";
    cin >> number;

    if (number < 0) {
        cout << "Factorial is not defined for negative numbers." << endl;
        return 0;
    }

    for (int i = 1; i <= number; i++) {
        factorial *= i;
    }

    cout << "Factorial of " << number
         << " = " << factorial << endl;

    return 0;
}
```

## Sample Output

```text
Enter a non-negative integer: 5
Factorial of 5 = 120
```
