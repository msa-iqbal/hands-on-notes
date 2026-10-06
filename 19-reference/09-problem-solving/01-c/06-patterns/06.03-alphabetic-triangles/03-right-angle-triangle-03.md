# Right Angle Alphabetic Triangle 03

## Problem

Print the following reverse alphabet pattern:

```text
E
ED
EDC
EDCB
EDCBA
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
        for (j = 0; j <= i; j++)
        {
            printf("%c", 'E' - j);
        }

        printf("\n");
    }

    return 0;
}
```

## Output

```text
Enter number of rows: 5

E
ED
EDC
EDCB
EDCBA
```
