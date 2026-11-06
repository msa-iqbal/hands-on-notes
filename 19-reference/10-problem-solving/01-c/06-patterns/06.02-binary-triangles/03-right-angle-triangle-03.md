# Right Angle Binary Triangle 03

## Problem

Print consecutive binary digits:

```text
1
01
010
1010
10101
```

## C Program

```c
#include <stdio.h>

int main(void)
{
    int i, j, n;
    int bit = 1;

    printf("Enter number of rows: ");
    scanf("%d", &n);

    for (i = 1; i <= n; i++)
    {
        for (j = 1; j <= i; j++)
        {
            printf("%d", bit);
            bit = 1 - bit;
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
010
1010
10101
```
