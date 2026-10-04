# Left Angle Number Triangle 05

## Problem

Print consecutive numbers starting from each row:

```text
    1
   23
  345
 4567
56789
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

        for (j = 0; j < i; j++)
        {
            printf("%d", i + j);
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
   23
  345
 4567
56789
```
