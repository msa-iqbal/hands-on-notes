# Positive or Negative

Write a C++ program to determine whether a number is positive, negative, or zero.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    double number;

    cout << "Enter a number: ";
    cin >> number;

    if (number > 0) {
        cout << number << " is positive." << endl;
    } else if (number < 0) {
        cout << number << " is negative." << endl;
    } else {
        cout << "The number is zero." << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter a number: -15
-15 is negative.
```
