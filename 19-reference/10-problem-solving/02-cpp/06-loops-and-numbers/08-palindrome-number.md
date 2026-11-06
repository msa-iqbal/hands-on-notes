# Palindrome Number

Write a C++ program to determine whether an integer is a palindrome.

A palindrome number reads the same from left to right and right to left.

## Program

```cpp
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
    long long reverse = 0;

    while (number > 0) {
        reverse = reverse * 10 + number % 10;
        number /= 10;
    }

    if (original == reverse) {
        cout << original << " is a palindrome number." << endl;
    } else {
        cout << original << " is not a palindrome number." << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter a non-negative integer: 1221
1221 is a palindrome number.
```
