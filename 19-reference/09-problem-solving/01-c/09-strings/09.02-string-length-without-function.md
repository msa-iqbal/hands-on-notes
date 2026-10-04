# String Length Without Using a Function

## Problem

Write a C program to find the length of a string without using the built-in `strlen()` function.

## C Program

```c
#include <stdio.h>

int main(void)
{
    char str[100];
    int length = 0;

    printf("Enter a string: ");
    fgets(str, sizeof(str), stdin);

    while (str[length] != '\0' && str[length] != '\n')
    {
        length++;
    }

    printf("String length = %d\n", length);

    return 0;
}
```

## Sample Output

```text
Enter a string: Hello World
String length = 11
```
