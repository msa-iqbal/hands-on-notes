# Count Capital Letters, Small Letters, and Digits

## Problem

Write a C program to count the number of uppercase letters, lowercase letters, and digits in a string.

## C Program

```c
#include <stdio.h>
#include <ctype.h>

int main(void)
{
    char str[200];
    int capital = 0;
    int small = 0;
    int digits = 0;

    printf("Enter a string: ");
    fgets(str, sizeof(str), stdin);

    for (int i = 0; str[i] != '\0'; i++)
    {
        unsigned char ch = (unsigned char)str[i];

        if (isupper(ch))
            capital++;
        else if (islower(ch))
            small++;
        else if (isdigit(ch))
            digits++;
    }

    printf("Capital letters = %d\n", capital);
    printf("Small letters = %d\n", small);
    printf("Digits = %d\n", digits);

    return 0;
}
```

## Sample Output

```text
Enter a string: Hello World 123
Capital letters = 2
Small letters = 8
Digits = 3
```
