# Pyramid Pattern 03

## Problem

Write a C program to print a centered pyramid where each row contains the same number.

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
            printf("%d ", i);

        printf("\n");
    }

    return 0;
}
```

## Sample Output

```text
Enter number of rows: 5
    1
   2 2
  3 3 3
 4 4 4 4
5 5 5 5 5
```
