# Armstrong Number

Write a C++ program to determine whether a number is an Armstrong number.

For an `n`-digit number, the sum of each digit raised to the power `n` must equal the original number.

## Program

```cpp
#include <cmath>
#include <iostream>
using namespace std;

int main() {
    long long number;

    cout << "Enter a non-negative integer: ";
    cin >> number;

    if (number < 0) {
        cout << "Please enter a non-negative integer." << endl;
        return 0;
    }

    long long original = number;
    long long temp = number;
    int digits = 0;

    if (number == 0) {
        digits = 1;
    } else {
        while (temp > 0) {
            digits++;
            temp /= 10;
        }
    }

    temp = number;
    long long sum = 0;

    while (temp > 0) {
        int digit = temp % 10;
        sum += static_cast<long long>(pow(digit, digits));
        temp /= 10;
    }

    if (original == 0) {
        sum = 0;
    }

    if (sum == original) {
        cout << original << " is an Armstrong number." << endl;
    } else {
        cout << original << " is not an Armstrong number." << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter a non-negative integer: 153
153 is an Armstrong number.
```
