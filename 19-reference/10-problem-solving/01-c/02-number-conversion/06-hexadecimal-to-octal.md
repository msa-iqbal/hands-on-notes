# Hexadecimal to Octal

Write a C program to convert a hexadecimal number to its octal equivalent.

## Program

```c
#include <stdio.h>

int main(void)
{
    char hexadecimal[100];
    int decimal = 0;
    int octal = 0;
    int base = 1;
    int value;
    int i = 0;

    printf("Enter a hexadecimal number: ");
    scanf("%99s", hexadecimal);

    while (hexadecimal[i] != '\0')
    {
        char ch = hexadecimal[i];

        if (ch >= '0' && ch <= '9')
        {
            value = ch - '0';
        }
        else if (ch >= 'A' && ch <= 'F')
        {
            value = ch - 'A' + 10;
        }
        else if (ch >= 'a' && ch <= 'f')
        {
            value = ch - 'a' + 10;
        }
        else
        {
            printf("Invalid hexadecimal number.\n");
            return 0;
        }

        decimal = decimal * 16 + value;
        i++;
    }

    if (decimal == 0)
    {
        printf("Octal = 0\n");
        return 0;
    }

    while (decimal > 0)
    {
        octal += (decimal % 8) * base;
        decimal /= 8;
        base *= 10;
    }

    printf("Octal = %d\n", octal);

    return 0;
}
```

## Output

```text
Enter a hexadecimal number: FF
Octal = 377
```
