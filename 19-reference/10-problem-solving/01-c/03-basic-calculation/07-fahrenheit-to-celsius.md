# Fahrenheit to Celsius

Write a C program to convert a temperature from Fahrenheit to Celsius.

## Program

```c
#include <stdio.h>

int main(void)
{
    double fahrenheit, celsius;

    printf("Enter temperature in Fahrenheit: ");
    scanf("%lf", &fahrenheit);

    celsius = (fahrenheit - 32.0) * 5.0 / 9.0;

    printf("Temperature in Celsius = %.2f\n", celsius);

    return 0;
}
```

## Output

```text
Enter temperature in Fahrenheit: 77
Temperature in Celsius = 25.00
```
