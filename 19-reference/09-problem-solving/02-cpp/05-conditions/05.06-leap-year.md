# Leap Year

Write a C++ program to determine whether a given year is a leap year.

A year is a leap year if:

- It is divisible by 400, or

- It is divisible by 4 but not divisible by 100.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int year;

    cout << "Enter a year: ";
    cin >> year;

    if (year <= 0) {
        cout << "Please enter a valid year." << endl;
    } else if ((year % 400 == 0) ||
               (year % 4 == 0 && year % 100 != 0)) {
        cout << year << " is a leap year." << endl;
    } else {
        cout << year << " is not a leap year." << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter a year: 2024
2024 is a leap year.
```
