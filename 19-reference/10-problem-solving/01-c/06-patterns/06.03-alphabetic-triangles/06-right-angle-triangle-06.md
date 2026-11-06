# Right Angle Alphabetic Triangle 06

## Problem

Print alternating alphabet characters in a triangular arrangement:

```text
A
BA
ABA
BABA
ABABA
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
            if ((i + j) % 2 == 0)
                printf("A");
            else
                printf("B");
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
ABA
BABA
ABABA
```
