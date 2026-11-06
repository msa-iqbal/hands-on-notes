# Capital or Small Letter

Write a C program to determine whether a given alphabetic character is a capital letter or a small letter.

## Program

```c
#include <stdio.h>

int main(void)
{
    char ch;

    printf("Enter an alphabet: ");
    scanf("%c", &ch);

    if (ch >= 'A' && ch <= 'Z')
    {
        printf("%c is a capital letter.\n", ch);
    }
    else if (ch >= 'a' && ch <= 'z')
    {
        printf("%c is a small letter.\n", ch);
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
Enter an alphabet: G
G is a capital letter.
```
