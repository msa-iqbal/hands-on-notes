# Square Series Sum

Write a C++ program to calculate:

```text
1² + 2² + 3² + ... + n²
```

Formula:

```text
n(n + 1)(2n + 1) / 6
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

    long long sum = n * (n + 1) * (2 * n + 1) / 6;

    cout << "Sum of squares = " << sum << endl;

    return 0;
}
```

## Sample Output

```text
Enter n: 5
Sum of squares = 55
```
