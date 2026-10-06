# Celsius to Fahrenheit

Write a C++ program to convert a temperature from Celsius to Fahrenheit.

Formula:

```text
Fahrenheit = (Celsius × 9/5) + 32
```

## Program

```cpp
#include <iomanip>
#include <iostream>
using namespace std;

int main() {
    double celsius;

    cout << "Enter temperature in Celsius: ";
    cin >> celsius;

    double fahrenheit = (celsius * 9.0 / 5.0) + 32.0;

    cout << fixed << setprecision(2);
    cout << "Temperature in Fahrenheit = "
         << fahrenheit << endl;

    return 0;
}
```

## Sample Output

```text
Enter temperature in Celsius: 25
Temperature in Fahrenheit = 77.00
```
