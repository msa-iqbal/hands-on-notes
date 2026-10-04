# Flow Pattern 04

## Problem

Write a C program to print a continuous even-number pattern in triangular form.

### C Program

```c
#include <stdio.h>

int main(void)
{
    int n;
    int number = 2;

    printf("Enter number of rows: ");
    scanf("%d", &n);

    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= i; j++)
        {
            printf("%d ", number);
            number += 2;
        }

        printf("\n");
    }

    return 0;
}
```

### Sample Output

```text
Enter number of rows: 5
2
4 6
8 10 12
14 16 18 20
22 24 26 28 30
```
