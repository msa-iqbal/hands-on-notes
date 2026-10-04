# Natural Number Series Sum

Write a C++ program to calculate the sum of the first `n` natural numbers.

Formula:

```text
1 + 2 + 3 + ... + n = n × (n + 1) / 2
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

    long long sum = n * (n + 1) / 2;

    cout << "Sum of natural numbers = " << sum << endl;

    return 0;
}
```

## Sample Output

```text
Enter n: 10
Sum of natural numbers = 55
```
