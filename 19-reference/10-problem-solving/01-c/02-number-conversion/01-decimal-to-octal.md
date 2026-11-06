# Decimal to Octal

Write a C program to convert a decimal number to its octal equivalent.

## Program

```c
#include <stdio.h>

int main(void)
{
    int decimal, octal = 0, place = 1, remainder;

    printf("Enter a decimal number: ");
    scanf("%d", &decimal);

    while (decimal > 0)
    {
        remainder = decimal % 8;
        octal += remainder * place;
        decimal /= 8;
        place *= 10;
    }

    printf("Octal = %d\n", octal);

    return 0;
}
```

## Output

```text
Enter a decimal number: 25
Octal = 31
```
