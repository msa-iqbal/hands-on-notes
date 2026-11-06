# Left Angle Alphabetic Triangle 02

## Problem

Print a right-aligned triangle with repeated alphabet characters:

```text
    A
   BB
  CCC
 DDDD
EEEEE
```

## C Program

```c
#include <stdio.h>

int main(void)
{
    int i, j, n;

    printf("Enter number of rows: ");
    scanf("%d", &n);

    for (i = 0; i < n; i++)
    {
        for (j = 0; j < n - i - 1; j++)
        {
            printf(" ");
        }

        for (j = 0; j <= i; j++)
        {
            printf("%c", 'A' + i);
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
   BB
  CCC
 DDDD
EEEEE
```
