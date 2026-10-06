# Left Angle Alphabetic Triangle 01

## Problem

Print a right-aligned alphabetic triangle:

```text
    A
   AB
  ABC
 ABCD
ABCDE
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
            printf("%c", 'A' + j);
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
   AB
  ABC
 ABCD
ABCDE
```
