# Pattern 01

## Problem

Write a C program to print a square pattern of `*` with the same number of rows and columns.

## C Program

```c
#include <stdio.h>

int main(void)
{
    int n;

    printf("Enter size of the square: ");
    scanf("%d", &n);

    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= n; j++)
        {
            printf("*");
        }

        printf("\n");
    }

    return 0;
}
```

## Sample Output

```text
Enter size of the square: 5
*****
*****
*****
*****
*****
```
