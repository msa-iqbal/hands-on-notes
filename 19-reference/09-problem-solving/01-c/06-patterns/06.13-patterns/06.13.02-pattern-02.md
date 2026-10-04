# Pattern 02 — Hollow Square

## Problem

Write a C program to print a hollow square pattern using `*`.

For `n = 5`, the output should be:

```text
*****
*   *
*   *
*   *
*****
```

## C Program

```c
#include <stdio.h>

int main(void)
{
    int n;

    printf("Enter the size of the square: ");
    scanf("%d", &n);

    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= n; j++)
        {
            if (i == 1 || i == n || j == 1 || j == n)
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
Enter the size of the square: 5
*****
*   *
*   *
*   *
*****
```
