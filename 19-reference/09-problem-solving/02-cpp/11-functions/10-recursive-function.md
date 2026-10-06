# Recursive Function

Write a C++ program to calculate the factorial of a number using a recursive function.

## Program

```cpp
#include <iostream>
using namespace std;

long long factorial(int n) {
    if (n <= 1) {
        return 1;
    }

    return n * factorial(n - 1);
}

int main() {
    int n;

    cout << "Enter a number: ";
    cin >> n;

    if (n < 0) {
        cout << "Factorial is not defined for negative numbers." << endl;
    } else {
        cout << "Factorial = " << factorial(n) << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter a number: 5
Factorial = 120
```
