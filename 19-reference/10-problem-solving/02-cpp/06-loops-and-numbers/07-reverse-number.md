# Reverse Number

Write a C++ program to reverse the digits of an integer.

## Program

```cpp
#include <cstdlib>
#include <iostream>
using namespace std;

int main() {
    long long number;

    cout << "Enter an integer: ";
    cin >> number;

    bool negative = number < 0;
    number = llabs(number);

    long long reverse = 0;

    while (number > 0) {
        reverse = reverse * 10 + number % 10;
        number /= 10;
    }

    if (negative) {
        reverse = -reverse;
    }

    cout << "Reversed number = " << reverse << endl;

    return 0;
}
```

## Sample Output

```text
Enter an integer: 12345
Reversed number = 54321
```
