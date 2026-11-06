# Armstrong Numbers in Range

Write a C++ program to print all Armstrong numbers within a given range.

## Program

```cpp
#include <cmath>
#include <iostream>
using namespace std;

bool isArmstrong(int number) {
    if (number < 0) {
        return false;
    }

    int original = number;
    int temp = number;
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
    int sum = 0;

    while (temp > 0) {
        int digit = temp % 10;
        sum += static_cast<int>(pow(digit, digits));
        temp /= 10;
    }

    return sum == original;
}

int main() {
    int start, end;

    cout << "Enter start and end: ";
    cin >> start >> end;

    cout << "Armstrong numbers: ";

    for (int number = start; number <= end; number++) {
        if (isArmstrong(number)) {
            cout << number << " ";
        }
    }

    cout << endl;

    return 0;
}
```

## Sample Output

```text
Enter start and end: 1 500
Armstrong numbers: 1 2 3 4 5 6 7 8 9 153 370 371 407
```
