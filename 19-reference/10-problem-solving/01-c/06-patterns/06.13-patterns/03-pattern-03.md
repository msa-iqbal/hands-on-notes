# Pattern 03 — Number Square

## Problem

Write a C program to print a square number pattern where each row contains the same number.

For `n = 5`, the output should be:

```text
11111
22222
33333
44444
55555
```

## C Program

```c
#include <stdio.h>

int main(void)
{
    int n;

    printf("Enter the size of the square: ");
    scanf("%d", &n);

    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= n; j++)
        {
            printf("%d", i);
        }

        printf("\n");
    }

    return 0;
}
```

## Sample Output

```text
Enter the size of the square: 5
11111
22222
33333
44444
55555
```
