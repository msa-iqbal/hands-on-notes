# Sum of Digits

Write a C++ program to calculate the sum of all digits of an integer.

## Program

```cpp
#include <cstdlib>
#include <iostream>
using namespace std;

int main() {
    long long number;

    cout << "Enter an integer: ";
    cin >> number;

    number = llabs(number);

    int sum = 0;

    while (number > 0) {
        sum += number % 10;
        number /= 10;
    }

    cout << "Sum of digits = " << sum << endl;

    return 0;
}
```

## Sample Output

```text
Enter an integer: 12345
Sum of digits = 15
```
