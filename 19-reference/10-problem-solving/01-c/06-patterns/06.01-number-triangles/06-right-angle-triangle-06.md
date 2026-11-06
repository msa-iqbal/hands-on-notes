# Right Angle Number Triangle 06

## Problem

Print a right-angle triangle where each row starts from the row number and increases:

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
