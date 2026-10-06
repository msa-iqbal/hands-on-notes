# Decimal to Hexadecimal

Write a C program to convert a decimal number to its hexadecimal equivalent.

## Program

```c
#include <stdio.h>

int main(void)
{
    int decimal, remainder;
    char hexadecimal[100];
    int i = 0;

    printf("Enter a decimal number: ");
    scanf("%d", &decimal);

    if (decimal == 0)
    {
        printf("Hexadecimal = 0\n");
        return 0;
    }

    while (decimal > 0)
    {
        remainder = decimal % 16;

        if (remainder < 10)
            hexadecimal[i] = remainder + '0';
        else
            hexadecimal[i] = remainder - 10 + 'A';

        decimal /= 16;
        i++;
    }

    printf("Hexadecimal = ");

    while (i > 0)
    {
        printf("%c", hexadecimal[--i]);
    }

    printf("\n");

    return 0;
}
```

## Output

```text
Enter a decimal number: 255
Hexadecimal = FF
```
