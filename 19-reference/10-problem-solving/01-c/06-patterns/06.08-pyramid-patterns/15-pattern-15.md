# Pyramid Pattern 15

## Problem

Write a C program to print a centered numeric pyramid where each row increases toward the center and then decreases.

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
            printf("%d", j);

        for (int j = 2; j <= i; j++)
            printf("%d", j);

        printf("\n");
    }

    return 0;
}
```

## Sample Output

```text
Enter number of rows: 5
    1
   212
  32123
 4321234
543212345
```
