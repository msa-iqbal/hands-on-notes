# Right Angle Binary Triangle 04

## Problem

Print a triangle where each row starts with `1` and alternates:

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
        for (j = 1; j <= i; j++)
        {
            printf("%d", (j + 1) % 2);
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
