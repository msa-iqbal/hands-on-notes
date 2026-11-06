# Print String Characters

## Problem

Write a C program to read a string and print each character separately.

## C Program

```c
#include <stdio.h>

int main(void)
{
    char str[100];

    printf("Enter a string: ");
    fgets(str, sizeof(str), stdin);

    printf("Characters:\n");

    for (int i = 0; str[i] != '\0' && str[i] != '\n'; i++)
    {
        printf("%c\n", str[i]);
    }

    return 0;
}
```

## Sample Output

```text
Enter a string: Hello
Characters:
H
e
l
l
o
```
