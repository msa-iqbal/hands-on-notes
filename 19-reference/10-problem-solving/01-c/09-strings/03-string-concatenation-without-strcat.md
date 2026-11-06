# String Concatenation Without Using strcat()

## Problem

Write a C program to concatenate two strings without using the built-in `strcat()` function.

## C Program

```c
#include <stdio.h>

int main(void)
{
    char first[200];
    char second[100];
    int i = 0;
    int j = 0;

    printf("Enter first string: ");
    fgets(first, sizeof(first), stdin);

    printf("Enter second string: ");
    fgets(second, sizeof(second), stdin);

    while (first[i] != '\0' && first[i] != '\n')
    {
        i++;
    }

    while (second[j] != '\0' && second[j] != '\n')
    {
        first[i] = second[j];
        i++;
        j++;
    }

    first[i] = '\0';

    printf("Concatenated string: %s\n", first);

    return 0;
}
```

## Sample Output

```text
Enter first string: Hello
Enter second string: World
Concatenated string: HelloWorld
```
