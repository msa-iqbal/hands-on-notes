# Harmonic Series Sum

Write a C++ program to calculate the sum of the first `n` terms of the harmonic series.

The series is:

```text
1 + 1/2 + 1/3 + ... + 1/n
```

## Program

```cpp
#include <iomanip>
#include <iostream>
using namespace std;

int main() {
    int n;

    cout << "Enter n: ";
    cin >> n;

    if (n < 1) {
        cout << "Please enter a positive integer." << endl;
        return 0;
    }

    double sum = 0.0;

    for (int i = 1; i <= n; i++) {
        sum += 1.0 / i;
    }

    cout << fixed << setprecision(6);
    cout << "Harmonic sum = " << sum << endl;

    return 0;
}
```

## Sample Output

```text
Enter n: 5
Harmonic sum = 2.283333
```
