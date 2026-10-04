# Uppercase to Lowercase Using Library Function

Write a C program to convert an uppercase character to lowercase using the `tolower()` library function.

## Program

```c
#include <stdio.h>
#include <ctype.h>

int main(void)
{
    char ch;

    printf("Enter an uppercase character: ");
    scanf("%c", &ch);

    if (isupper((unsigned char)ch))
    {
        ch = tolower((unsigned char)ch);
        printf("Lowercase = %c\n", ch);
    }
    else
    {
        printf("Invalid input.\n");
    }

    return 0;
}
```

## Output

```text
Enter an uppercase character: M
Lowercase = m
```
