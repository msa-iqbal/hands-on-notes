# Left Angle Binary Triangle 06

## Problem

Print a right-aligned binary triangle based on the column position:

```text
    1
   10
  101
 1010
10101
```

## C Program

```c
#include <stdio.h>

int main(void)
{
    int i, j, n;

    printf("Enter number of rows: ");
    scanf("%d", &n);

    for (i = 1; i <= n; i++)
    {
        for (j = 1; j <= n - i; j++)
            printf(" ");

        for (j = 1; j <= i; j++)
        {
            if (j % 2 == 1)
                printf("1");
            else
                printf("0");
        }

        printf("\n");
    }

    return 0;
}
```

## Example Output

```text
Enter number of rows: 5

    1
   10
  101
 1010
10101
```
