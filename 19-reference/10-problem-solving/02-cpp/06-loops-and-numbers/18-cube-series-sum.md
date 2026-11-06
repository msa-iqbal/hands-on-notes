# Cube Series Sum

Write a C++ program to calculate:

```text
1³ + 2³ + 3³ + ... + n³
```

Formula:

```text
[n(n + 1) / 2]²
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
    sum *= sum;

    cout << "Sum of cubes = " << sum << endl;

    return 0;
}
```

## Sample Output

```text
Enter n: 5
Sum of cubes = 225
```
