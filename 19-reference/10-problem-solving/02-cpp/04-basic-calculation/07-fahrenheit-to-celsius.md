# Fahrenheit to Celsius

Write a C++ program to convert a temperature from Fahrenheit to Celsius.

Formula:

```text
Celsius = (Fahrenheit - 32) × 5/9
```

## Program

```cpp
#include <iomanip>
#include <iostream>
using namespace std;

int main() {
    double fahrenheit;

    cout << "Enter temperature in Fahrenheit: ";
    cin >> fahrenheit;

    double celsius = (fahrenheit - 32.0) * 5.0 / 9.0;

    cout << fixed << setprecision(2);
    cout << "Temperature in Celsius = "
         << celsius << endl;

    return 0;
}
```

## Sample Output

```text
Enter temperature in Fahrenheit: 77
Temperature in Celsius = 25.00
```
