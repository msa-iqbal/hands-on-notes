# Pyramid Pattern 12

## Problem

Write a C program to print a centered pyramid of descending alphabets.

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

        for (int j = i; j >= 1; j--)
            printf("%c ", 'A' + j - 1);

        printf("\n");
    }

    return 0;
}
```

## Sample Output

```text
Enter number of rows: 5
    A
   B A
  C B A
 D C B A
E D C B A
```
