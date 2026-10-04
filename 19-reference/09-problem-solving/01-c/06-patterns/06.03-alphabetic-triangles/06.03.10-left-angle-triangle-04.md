# Left Angle Alphabetic Triangle 04

## Problem

Print a right-aligned reverse alphabet pattern:

```text
    A
   BA
  CBA
 DCBA
EDCBA
```

## C Program

```c
#include <stdio.h>

int main(void)
{
    int i, j, n;

    printf("Enter number of rows: ");
    scanf("%d", &n);

    for (i = 0; i < n; i++)
    {
        for (j = 0; j < n - i - 1; j++)
        {
            printf(" ");
        }

        for (j = i; j >= 0; j--)
        {
            printf("%c", 'A' + j);
        }

        printf("\n");
    }

    return 0;
}
```

## Output

```text
Enter number of rows: 5

    A
   BA
  CBA
 DCBA
EDCBA
```
