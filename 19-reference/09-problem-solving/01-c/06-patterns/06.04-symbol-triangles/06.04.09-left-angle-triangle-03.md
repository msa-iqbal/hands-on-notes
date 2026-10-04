# Left-Angle Symbol Triangle 03

## Problem

Write a C program to print a right-aligned triangle using alternating `*` and `#` symbols.

### C Program

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
        {
            printf(" ");
        }

        for (int j = 1; j <= i; j++)
        {
            if ((i + j) % 2 == 0)
                printf("*");
            else
                printf("#");
        }

        printf("\n");
    }

    return 0;
}
```

### Sample Output

```text
Enter number of rows: 5
    *
   #*
  *#*
 #*#*
*#*#*
```
