# Count Digit in Integer

Write a C++ program to count the number of digits in an integer.

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

    int count = 0;

    if (number == 0) {
        count = 1;
    } else {
        while (number > 0) {
            count++;
            number /= 10;
        }
    }

    cout << "Number of digits = " << count << endl;

    return 0;
}
```

## Sample Output

```text
Enter an integer: 123456
Number of digits = 6
```
