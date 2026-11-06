# Prime Numbers in Range

Write a C++ program to print all prime numbers within a given range.

## Program

```cpp
#include <iostream>
using namespace std;

bool isPrime(int number) {
    if (number < 2) {
        return false;
    }

    for (int i = 2; i <= number / i; i++) {
        if (number % i == 0) {
            return false;
        }
    }

    return true;
}

int main() {
    int start, end;

    cout << "Enter start and end: ";
    cin >> start >> end;

    cout << "Prime numbers: ";

    for (int number = start; number <= end; number++) {
        if (isPrime(number)) {
            cout << number << " ";
        }
    }

    cout << endl;

    return 0;
}
```

## Sample Output

```text
Enter start and end: 10 30
Prime numbers: 11 13 17 19 23 29
```
