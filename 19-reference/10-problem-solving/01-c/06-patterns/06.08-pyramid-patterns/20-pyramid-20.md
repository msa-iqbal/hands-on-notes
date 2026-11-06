# Hollow Inverted Pyramid Pattern 20

## Problem

Write a C program to print a hollow inverted centered pyramid using `*`.

## C Program

```c
#include <stdio.h>

int main(void)
{
    int n;

    printf("Enter number of rows: ");
    scanf("%d", &n);

    for (int i = n; i >= 1; i--)
    {
        int spaces = n - i;

        for (int j = 1; j <= spaces; j++)
            printf(" ");

        for (int j = 1; j <= 2 * i - 1; j++)
        {
            if (i == n || i == 1 || j == 1 || j == 2 * i - 1)
                printf("*");
            else
                printf(" ");
        }

        printf("\n");
    }

    return 0;
}
```

## Sample Output

```text
Enter number of rows: 5
*********
 *     *
  *   *
   * *
    *
```
