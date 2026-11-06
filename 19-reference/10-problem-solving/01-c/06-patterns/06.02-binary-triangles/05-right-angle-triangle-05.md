# Right Angle Binary Triangle 05

## Problem

Print a binary triangle where each row starts according to the row number:

```text
1
00
111
0000
11111
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
            printf("%d", (i + 1) % 2);
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
00
111
0000
11111
```
