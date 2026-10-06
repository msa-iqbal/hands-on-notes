# Pyramid Pattern 06

## Problem

Write a C program to print a centered pyramid with descending numbers in each row.

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
            printf("%d ", j);

        printf("\n");
    }

    return 0;
}
```

## Sample Output

```text
Enter number of rows: 5
    1
   2 1
  3 2 1
 4 3 2 1
5 4 3 2 1
```
