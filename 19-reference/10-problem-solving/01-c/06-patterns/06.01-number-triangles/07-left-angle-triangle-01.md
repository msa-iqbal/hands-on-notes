# Left Angle Number Triangle 01

## Problem

Print a left-aligned number triangle:

```text
    1
   12
  123
 1234
12345
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
        {
            printf(" ");
        }

        for (j = 1; j <= i; j++)
        {
            printf("%d", j);
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
   12
  123
 1234
12345
```
