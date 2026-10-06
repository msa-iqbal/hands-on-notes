# GCD and LCM

Write a C++ program to find the Greatest Common Divisor (GCD) and Least Common Multiple (LCM) of two integers.

## Program

```cpp
#include <cstdlib>
#include <iostream>
using namespace std;

int main() {
    long long a, b;

    cout << "Enter two integers: ";
    cin >> a >> b;

    if (a == 0 && b == 0) {
        cout << "GCD and LCM are undefined for 0 and 0." << endl;
        return 0;
    }

    long long x = llabs(a);
    long long y = llabs(b);

    while (y != 0) {
        long long remainder = x % y;
        x = y;
        y = remainder;
    }

    long long gcd = x;
    long long lcm = (a == 0 || b == 0)
                        ? 0
                        : llabs((a / gcd) * b);

    cout << "GCD = " << gcd << endl;
    cout << "LCM = " << lcm << endl;

    return 0;
}
```

## Sample Output

```text
Enter two integers: 12 18
GCD = 6
LCM = 36
```
