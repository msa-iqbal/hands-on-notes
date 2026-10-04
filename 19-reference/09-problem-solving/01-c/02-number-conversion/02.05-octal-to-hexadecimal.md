# Octal to Hexadecimal

Write a C program to convert an octal number to its hexadecimal equivalent.

## Program

```c
#include <stdio.h>

int main(void)
{
    int octal, decimal = 0, base = 1;
    int remainder;

    printf("Enter an octal number: ");
    scanf("%d", &octal);

    while (octal > 0)
    {
        remainder = octal % 10;

        if (remainder >= 8)
        {
            printf("Invalid octal number.\n");
            return 0;
        }

        decimal += remainder * base;
        octal /= 10;
        base *= 8;
    }

    printf("Hexadecimal = %X\n", decimal);

    return 0;
}
```

## Output

```text
Enter an octal number: 377
Hexadecimal = FF
```
