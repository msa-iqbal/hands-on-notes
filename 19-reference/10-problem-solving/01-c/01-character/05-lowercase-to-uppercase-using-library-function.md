# Lowercase to Uppercase Using Library Function

Write a C program to convert a lowercase character to uppercase using the `toupper()` library function.

## Program

```c
#include <stdio.h>
#include <ctype.h>

int main(void)
{
    char ch;

    printf("Enter a lowercase character: ");
    scanf("%c", &ch);

    if (islower((unsigned char)ch))
    {
        ch = toupper((unsigned char)ch);
        printf("Uppercase = %c\n", ch);
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
Enter a lowercase character: m
Uppercase = M
```
