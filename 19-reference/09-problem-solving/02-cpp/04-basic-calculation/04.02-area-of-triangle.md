# Area of Triangle

Write a C++ program to calculate the area of a triangle using its base and height.

Formula:

```text
Area = 1/2 × base × height
```

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    double base, height;

    cout << "Enter base: ";
    cin >> base;

    cout << "Enter height: ";
    cin >> height;

    double area = 0.5 * base * height;

    cout << "Area of triangle = " << area << endl;

    return 0;
}
```

## Sample Output

```text
Enter base: 10
Enter height: 6
Area of triangle = 30
```
