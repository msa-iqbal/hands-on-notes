# Area of Rectangle

Write a C++ program to calculate the area of a rectangle using its length and width.

Formula:

```text
Area = length × width
```

## Program

```cpp
#include <iostream>
using namespace std;

int main() {
    double length, width;

    cout << "Enter length: ";
    cin >> length;

    cout << "Enter width: ";
    cin >> width;

    double area = length * width;

    cout << "Area of rectangle = " << area << endl;

    return 0;
}
```

## Sample Output

```text
Enter length: 12
Enter width: 5
Area of rectangle = 60
```
