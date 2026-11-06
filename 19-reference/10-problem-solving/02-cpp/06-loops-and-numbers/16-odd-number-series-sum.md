# Odd Number Series Sum

Write a C++ program to calculate the sum of the first `n` odd numbers.

The series is:

```text
1 + 3 + 5 + ... + (2n - 1)
```

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    long long n;

    cout << "Enter n: ";
    cin >> n;

    if (n < 1) {
        cout << "Please enter a positive integer." << endl;
        return 0;
    }

    long long sum = n * n;

    cout << "Sum of first " << n
         << " odd numbers = " << sum << endl;

    return 0;
}
```

## Sample Output

```text
Enter n: 5
Sum of first 5 odd numbers = 25
```
