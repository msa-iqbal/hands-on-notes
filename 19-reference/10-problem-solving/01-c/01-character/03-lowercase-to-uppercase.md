# Lowercase to Uppercase

Write a C program to convert a lowercase character to uppercase without using a library function.

## Program

```c
#include <stdio.h>

int main(void)
{
    char ch;

    printf("Enter a lowercase character: ");
    scanf("%c", &ch);

    if (ch >= 'a' && ch <= 'z')
    {
        ch = ch - 'a' + 'A';
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
Enter a lowercase character: g
Uppercase = G
```
