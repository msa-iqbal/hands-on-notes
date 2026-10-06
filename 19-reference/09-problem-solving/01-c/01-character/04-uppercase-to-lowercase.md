# Uppercase to Lowercase

Write a C program to convert an uppercase character to lowercase without using a library function.

## Program

```c
#include <stdio.h>

int main(void)
{
    char ch;

    printf("Enter an uppercase character: ");
    scanf("%c", &ch);

    if (ch >= 'A' && ch <= 'Z')
    {
        ch = ch - 'A' + 'a';
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
Enter an uppercase character: G
Lowercase = g
```
