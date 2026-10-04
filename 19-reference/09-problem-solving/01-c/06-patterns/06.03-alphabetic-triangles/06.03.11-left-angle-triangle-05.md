# Left Angle Alphabetic Triangle 05

## Problem

Print consecutive alphabets in a right-aligned triangle:

```text
    A
   BC
  DEF
 GHIJ
KLMNO
```

## C Program

```c
#include <stdio.h>

int main(void)
{
    int i, j, n;
    char ch = 'A';

    printf("Enter number of rows: ");
    scanf("%d", &n);

    for (i = 1; i <= n; i++)
    {
        for (j = 1; j <= n - i; j++)
        {
            printf(" ");
        }

        for (j = 1; j <= i; j++)
        {
            printf("%c", ch);
            ch++;

            if (ch > 'Z')
                ch = 'A';
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
   BC
  DEF
 GHIJ
KLMNO
```
