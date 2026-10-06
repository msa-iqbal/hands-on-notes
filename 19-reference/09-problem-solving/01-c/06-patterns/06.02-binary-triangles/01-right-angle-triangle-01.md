# Right Angle Binary Triangle 01

## Problem

Print a right-angle triangle using alternating `0` and `1`:

```text
1
01
101
0101
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
            printf("%d", (i + j) % 2);
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
01
101
0101
10101
```
