# Strong Number

Write a C++ program to determine whether a number is a strong number.

A strong number is a number whose sum of the factorials of its digits equals the original number.

For example:

```text
145 = 1! + 4! + 5!
   = 1 + 24 + 120
   = 145
```

## Program

```cpp
#include <iostream>
using namespace std;

int factorial(int number) {
    int result = 1;

    for (int i = 2; i <= number; i++) {
        result *= i;
    }

    return result;
}

int main() {
    int number;

    cout << "Enter a non-negative integer: ";
    cin >> number;

    if (number < 0) {
        cout << "Please enter a non-negative integer." << endl;
        return 0;
    }

    int original = number;
    int sum = 0;

    if (number == 0) {
        sum = factorial(0);
    } else {
        while (number > 0) {
            int digit = number % 10;
            sum += factorial(digit);
            number /= 10;
        }
    }

    if (sum == original) {
        cout << original << " is a strong number." << endl;
    } else {
        cout << original << " is not a strong number." << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter a non-negative integer: 145
145 is a strong number.
```
