# Alternating Series Sum

Write a C++ program to calculate the sum of the alternating series:

```text
1 - 2 + 3 - 4 + 5 - 6 + ... ± n
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

    long long sum = 0;

    for (long long i = 1; i <= n; i++) {
        if (i % 2 == 0) {
            sum -= i;
        } else {
            sum += i;
        }
    }

    cout << "Alternating series sum = " << sum << endl;

    return 0;
}
```

## Sample Output

```text
Enter n: 10
Alternating series sum = -5
```
