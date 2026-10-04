# Prime Number

Write a C++ program to determine whether a given number is prime.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int number;
    bool isPrime = true;

    cout << "Enter an integer: ";
    cin >> number;

    if (number < 2) {
        isPrime = false;
    } else {
        for (int i = 2; i <= number / i; i++) {
            if (number % i == 0) {
                isPrime = false;
                break;
            }
        }
    }

    if (isPrime) {
        cout << number << " is a prime number." << endl;
    } else {
        cout << number << " is not a prime number." << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter an integer: 29
29 is a prime number.
```
