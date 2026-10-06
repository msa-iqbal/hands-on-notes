# Reverse String Using strrev()

## Problem

Write a C program to reverse a string using the `strrev()` function.

> Note: `strrev()` is not part of the ISO C standard and may not be available on all compilers.

## C Program

```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char str[100];

    printf("Enter a string: ");
    fgets(str, sizeof(str), stdin);

    str[strcspn(str, "\n")] = '\0';

    printf("Original string: %s\n", str);

    strrev(str);

    printf("Reversed string: %s\n", str);

    return 0;
}
```

## Sample Output

```text
Enter a string: Hello
Original string: Hello
Reversed string: olleH
```
