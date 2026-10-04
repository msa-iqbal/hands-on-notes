# Flow Pattern 03

## Problem

Write a C program to print a continuous odd-number pattern in triangular form.

### C Program

```c
#include <stdio.h>

int main(void)
{
    int n;
    int number = 1;

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
1
3 5
7 9 11
13 15 17 19
21 23 25 27 29
```
