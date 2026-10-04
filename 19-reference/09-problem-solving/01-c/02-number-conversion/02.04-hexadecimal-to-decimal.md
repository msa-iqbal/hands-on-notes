# Hexadecimal to Decimal

Write a C program to convert a hexadecimal number to its decimal equivalent.

## Program

```c
#include <stdio.h>
#include <ctype.h>

int main(void)
{
    char hexadecimal[100];
    int decimal = 0;
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

    printf("Decimal = %d\n", decimal);

    return 0;
}
```

## Output

```text
Enter a hexadecimal number: FF
Decimal = 255
```
