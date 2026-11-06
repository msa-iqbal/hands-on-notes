# Inverted Number Pyramid 18

## Problem

Write a C program to print an inverted pyramid of numbers.

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
        for (int j = 1; j <= n - i; j++)
            printf(" ");

        for (int j = 1; j <= i; j++)
            printf("%d ", j);

        printf("\n");
    }

    return 0;
}
```

## Sample Output

```text
Enter number of rows: 5
1 2 3 4 5
 1 2 3 4
  1 2 3
   1 2
    1
```
