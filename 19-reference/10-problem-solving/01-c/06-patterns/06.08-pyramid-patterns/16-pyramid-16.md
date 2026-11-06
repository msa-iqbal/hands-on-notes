# Pyramid Pattern 16

## Problem

Write a C program to print a centered pyramid using `#`.

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

        for (int j = 1; j <= 2 * i - 1; j++)
            printf("#");

        printf("\n");
    }

    return 0;
}
```

## Sample Output

```text
Enter number of rows: 5
    #
   ###
  #####
 #######
#########
```
