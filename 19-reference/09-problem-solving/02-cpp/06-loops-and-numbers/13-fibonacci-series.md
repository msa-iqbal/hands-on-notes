# Fibonacci Series

Write a C++ program to print the first `n` terms of the Fibonacci series.

The series starts with:

```text
0 1 1 2 3 5 8 ...
```

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;

    cout << "Enter number of terms: ";
    cin >> n;

    if (n <= 0) {
        cout << "Number of terms must be positive." << endl;
        return 0;
    }

    long long first = 0;
    long long second = 1;

    cout << "Fibonacci series: ";

    for (int i = 1; i <= n; i++) {
        cout << first << " ";

        long long next = first + second;
        first = second;
        second = next;
    }

    cout << endl;

    return 0;
}
```

## Sample Output

```text
Enter number of terms: 10
Fibonacci series: 0 1 1 2 3 5 8 13 21 34
```
