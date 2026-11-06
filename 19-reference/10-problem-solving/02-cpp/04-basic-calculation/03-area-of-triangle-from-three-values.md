# Area of Triangle from Three Values

Write a C++ program to calculate the area of a triangle when its three sides are given.

Use Heron's formula:

```text
s = (a + b + c) / 2

Area = √(s × (s - a) × (s - b) × (s - c))
```

## Program

```cpp
#include <cmath>
#include <iostream>
using namespace std;

int main() {
    double a, b, c;

    cout << "Enter three sides: ";
    cin >> a >> b >> c;

    if (a <= 0 || b <= 0 || c <= 0 ||
        a + b <= c || a + c <= b || b + c <= a) {
        cout << "Invalid triangle." << endl;
        return 0;
    }

    double s = (a + b + c) / 2.0;
    double area = sqrt(s * (s - a) * (s - b) * (s - c));

    cout << "Area of triangle = " << area << endl;

    return 0;
}
```

## Sample Output

```text
Enter three sides: 3 4 5
Area of triangle = 6
```
