# Celsius to Fahrenheit

Write a C program to convert a temperature from Celsius to Fahrenheit.

## Program

```c
#include <stdio.h>

int main(void)
{
    double celsius, fahrenheit;

    printf("Enter temperature in Celsius: ");
    scanf("%lf", &celsius);

    fahrenheit = (celsius * 9.0 / 5.0) + 32.0;

    printf("Temperature in Fahrenheit = %.2f\n", fahrenheit);

    return 0;
}
```

## Output

```text
Enter temperature in Celsius: 25
Temperature in Fahrenheit = 77.00
```
