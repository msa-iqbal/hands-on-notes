# Ternary Operator

Write a C++ program to determine whether a number is even or odd using the ternary operator.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int number;

    cout << "Enter an integer: ";
    cin >> number;

    string result = (number % 2 == 0) ? "even" : "odd";

    cout << number << " is " << result << "." << endl;

    return 0;
}
```

## Sample Output

```text
Enter an integer: 17
17 is odd.
```
