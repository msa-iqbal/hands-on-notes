# Even Number Series Sum

Write a C++ program to calculate the sum of the first `n` even numbers.

The series is:

```text
2 + 4 + 6 + ... + 2n
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

    long long sum = n * (n + 1);

    cout << "Sum of first " << n
         << " even numbers = " << sum << endl;

    return 0;
}
```

## Sample Output

```text
Enter n: 5
Sum of first 5 even numbers = 30
```
