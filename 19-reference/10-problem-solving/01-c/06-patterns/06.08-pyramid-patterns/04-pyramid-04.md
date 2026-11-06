# Pyramid Pattern 04

## Problem

Write a C program to print a centered alphabetic pyramid where each row contains the same character.

## C Program

```c
#include <stdio.h>

int main(void)
{
    int n;

    printf("Enter number of rows: ");
    scanf("%d", &n);

    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= n - i; j++)
            printf(" ");

        for (int j = 1; j <= i; j++)
            printf("%c ", 'A' + i - 1);

        printf("\n");
    }

    return 0;
}
```

## Sample Output

```text
Enter number of rows: 5
    A
   B B
  C C C
 D D D D
E E E E E
```
