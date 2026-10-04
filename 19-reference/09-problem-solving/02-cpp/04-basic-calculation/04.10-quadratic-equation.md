# Quadratic Equation

Write a C++ program to find the roots of a quadratic equation:

```text
ax² + bx + c = 0
```

The discriminant is:

```text
D = b² - 4ac
```

The roots are:

```text
x₁ = (-b + √D) / 2a
x₂ = (-b - √D) / 2a
```

## Program

```cpp
#include <cmath>
#include <iomanip>
#include <iostream>
using namespace std;

int main() {
    double a, b, c;

    cout << "Enter coefficients a, b, and c: ";
    cin >> a >> b >> c;

    if (a == 0) {
        cout << "This is not a quadratic equation." << endl;
        return 0;
    }

    double discriminant = b * b - 4.0 * a * c;

    cout << fixed << setprecision(2);

    if (discriminant > 0) {
        double x1 = (-b + sqrt(discriminant)) / (2.0 * a);
        double x2 = (-b - sqrt(discriminant)) / (2.0 * a);

        cout << "Two real roots:" << endl;
        cout << "x1 = " << x1 << endl;
        cout << "x2 = " << x2 << endl;
    } else if (discriminant == 0) {
        double x = -b / (2.0 * a);

        cout << "One repeated real root:" << endl;
        cout << "x = " << x << endl;
    } else {
        double realPart = -b / (2.0 * a);
        double imaginaryPart =
            sqrt(-discriminant) / (2.0 * a);

        cout << "Complex roots:" << endl;
        cout << "x1 = " << realPart << " + "
             << imaginaryPart << "i" << endl;
        cout << "x2 = " << realPart << " - "
             << imaginaryPart << "i" << endl;
    }

    return 0;
}
```

## Sample Output

```text
Enter coefficients a, b, and c: 1 -5 6
Two real roots:
x1 = 3.00
x2 = 2.00
```
