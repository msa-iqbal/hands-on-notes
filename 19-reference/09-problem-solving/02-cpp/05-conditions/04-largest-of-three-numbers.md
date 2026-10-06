# Largest of Three Numbers

Write a C++ program to find the largest among three numbers.

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    double a, b, c;

    cout << "Enter three numbers: ";
    cin >> a >> b >> c;

    double largest = a;

    if (b > largest) {
        largest = b;
    }

    if (c > largest) {
        largest = c;
    }

    cout << "Largest number = " << largest << endl;

    return 0;
}
```

## Sample Output

```text
Enter three numbers: 25 42 18
Largest number = 42
```
