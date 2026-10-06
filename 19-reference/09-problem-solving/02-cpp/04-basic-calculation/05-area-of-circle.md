# Area of Circle

Write a C++ program to calculate the area of a circle using its radius.

Formula:

```text
Area = π × radius²
```

## Program

```cpp
#include <iomanip>
#include <iostream>
using namespace std;

int main() {
    double radius;

    cout << "Enter radius: ";
    cin >> radius;

    if (radius < 0) {
        cout << "Radius cannot be negative." << endl;
        return 0;
    }

    constexpr double PI = 3.14159265358979323846;

    double area = PI * radius * radius;

    cout << fixed << setprecision(2);
    cout << "Area of circle = " << area << endl;

    return 0;
}
```

## Sample Output

```text
Enter radius: 5
Area of circle = 78.54
```
