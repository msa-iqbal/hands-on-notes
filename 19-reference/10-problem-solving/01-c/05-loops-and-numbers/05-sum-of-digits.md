# Sum of Digits

Write a C program to calculate the sum of all digits of an integer.

## Program

```c
#include <stdio.h>

int main(void)
{
    int number, digit, sum = 0;

    printf("Enter an integer: ");
    scanf("%d", &number);

    if (number < 0)
        number = -number;

    while (number != 0)
    {
        digit = number % 10;
        sum += digit;
        number /= 10;
    }

    printf("Sum of digits = %d\n", sum);

    return 0;
}
```

## Output

```text
Enter an integer: 12345
Sum of digits = 15
```
