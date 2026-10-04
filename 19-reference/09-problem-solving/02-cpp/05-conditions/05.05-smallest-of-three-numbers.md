# Smallest of Three Numbers

Write a C++ program to find the smallest among three numbers.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    double a, b, c;

    cout << "Enter three numbers: ";
    cin >> a >> b >> c;

    double smallest = a;

    if (b < smallest) {
        smallest = b;
    }

    if (c < smallest) {
        smallest = c;
    }

    cout << "Smallest number = " << smallest << endl;

    return 0;
}
```

## Sample Output

```text
Enter three numbers: 25 42 18
Smallest number = 18
```
